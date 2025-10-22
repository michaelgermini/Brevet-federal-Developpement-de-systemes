# Chapitre 10 : Sécurité logicielle

> **Objectif:** Maîtriser les principes fondamentaux de la sécurité logicielle, identifier et corriger les vulnérabilités courantes, et implémenter des mesures de protection robustes.

---

## 📋 Table des matières

1. [Vulnérabilités courantes (OWASP Top 10)](#101-vulnérabilités-courantes-owasp-top-10)
2. [Validation des entrées et gestion des sessions](#102-validation-des-entrées-et-gestion-des-sessions)
3. [Chiffrement et stockage sécurisé des données](#103-chiffrement-et-stockage-sécurisé-des-données)
4. [Audit de sécurité et pentests](#104-audit-de-sécurité-et-pentests)

---

## 10.1 Vulnérabilités courantes (OWASP Top 10)

### OWASP Top 10 (2021)

**A01: Broken Access Control**

```javascript
// ❌ Vulnérable - Pas de vérification d'autorisation
app.get('/api/users/:id/orders', async (req, res) => {
    const orders = await Order.find({ userId: req.params.id });
    res.json(orders);
});

// ✅ Sécurisé - Vérification que l'utilisateur accède à ses propres données
app.get('/api/users/:id/orders', authenticate, async (req, res) => {
    if (req.user.id !== parseInt(req.params.id) && !req.user.isAdmin) {
        return res.status(403).json({ error: 'Forbidden' });
    }
    const orders = await Order.find({ userId: req.params.id });
    res.json(orders);
});
```

**A02: Cryptographic Failures**

```javascript
// ❌ Mot de passe en clair
await User.create({ email, password });

// ✅ Hashage sécurisé avec bcrypt
const bcrypt = require('bcrypt');
const saltRounds = 12;
const hashedPassword = await bcrypt.hash(password, saltRounds);
await User.create({ email, passwordHash: hashedPassword });

// Vérification
const isValid = await bcrypt.compare(password, user.passwordHash);
```

**A03: Injection (SQL, NoSQL, Command)**

```javascript
// ❌ SQL Injection
const query = `SELECT * FROM users WHERE email = '${email}'`;

// ✅ Requête paramétrée
const query = 'SELECT * FROM users WHERE email = $1';
const result = await db.query(query, [email]);

// ❌ NoSQL Injection
await User.find({ email: req.body.email });  // Si email = { $ne: null }

// ✅ Validation et sanitization
const email = validator.escape(req.body.email);
if (!validator.isEmail(email)) {
    return res.status(400).json({ error: 'Invalid email' });
}
await User.find({ email });
```

**A04: Insecure Design**

Conception sécurisée dès le départ:
- ✅ Principe du moindre privilège
- ✅ Defense in depth (sécurité en couches)
- ✅ Fail securely (échec sécurisé)
- ✅ Validation côté serveur (jamais uniquement client)

**A05: Security Misconfiguration**

```yaml
# ❌ Configuration dangereuse
DEBUG=true
ENABLE_CORS=*
DISABLE_RATE_LIMIT=true

# ✅ Configuration sécurisée
DEBUG=false
ALLOWED_ORIGINS=https://myapp.com
RATE_LIMIT=100/hour
HIDE_ERROR_DETAILS=true
```

**A06: Vulnerable and Outdated Components**

```bash
# Audit des dépendances
npm audit
npm audit fix

# Garder à jour
npm update
npm outdated

# Utiliser Dependabot, Snyk, ou OWASP Dependency-Check
```

**A07: Identification and Authentication Failures**

```javascript
// ✅ Bonnes pratiques
// - Politique de mots de passe forts
const passwordSchema = new passwordValidator();
passwordSchema
    .is().min(12)
    .has().uppercase()
    .has().lowercase()
    .has().digits()
    .has().symbols()
    .has().not().spaces();

// - Multi-factor authentication
const speakeasy = require('speakeasy');
const secret = speakeasy.generateSecret({ length: 20 });

const token = speakeasy.totp({
    secret: secret.base32,
    encoding: 'base32'
});

const verified = speakeasy.totp.verify({
    secret: secret.base32,
    encoding: 'base32',
    token: userProvidedToken,
    window: 2
});

// - Limitation des tentatives de connexion
const loginAttempts = await redis.incr(`login_attempts:${email}`);
if (loginAttempts > 5) {
    await redis.expire(`login_attempts:${email}`, 900);  // 15 min
    return res.status(429).json({ error: 'Too many attempts' });
}
```

**A08: Software and Data Integrity Failures**

```javascript
// ✅ Vérification d'intégrité
const crypto = require('crypto');

function generateChecksum(data) {
    return crypto.createHash('sha256').update(data).digest('hex');
}

function verifyIntegrity(data, expectedChecksum) {
    const actualChecksum = generateChecksum(data);
    return actualChecksum === expectedChecksum;
}
```

**A09: Security Logging and Monitoring Failures**

```javascript
const winston = require('winston');

const logger = winston.createLogger({
    level: 'info',
    format: winston.format.json(),
    transports: [
        new winston.transports.File({ filename: 'security.log' })
    ]
});

// Logger les événements de sécurité
logger.warn('Failed login attempt', {
    email,
    ip: req.ip,
    userAgent: req.headers['user-agent'],
    timestamp: new Date()
});

logger.error('Unauthorized access attempt', {
    userId: req.user?.id,
    resource: req.path,
    method: req.method,
    ip: req.ip
});
```

**A10: Server-Side Request Forgery (SSRF)**

```javascript
// ❌ Vulnérable SSRF
app.post('/fetch-url', async (req, res) => {
    const response = await axios.get(req.body.url);
    res.json(response.data);
});

// ✅ Protection SSRF
const allowedHosts = ['api.example.com', 'cdn.example.com'];

app.post('/fetch-url', async (req, res) => {
    const url = new URL(req.body.url);
    
    // Whitelist des domaines
    if (!allowedHosts.includes(url.hostname)) {
        return res.status(400).json({ error: 'Invalid host' });
    }
    
    // Bloquer les IPs privées
    const ip = await dns.resolve4(url.hostname);
    if (isPrivateIP(ip[0])) {
        return res.status(400).json({ error: 'Private IP not allowed' });
    }
    
    const response = await axios.get(url.href);
    res.json(response.data);
});
```

---

## 10.2 Validation des entrées et gestion des sessions

### 10.2.1 Validation des entrées

**Côté serveur (obligatoire):**

```javascript
const Joi = require('joi');

const userSchema = Joi.object({
    email: Joi.string().email().required(),
    password: Joi.string().min(12).required(),
    name: Joi.string().min(2).max(100).required(),
    age: Joi.number().integer().min(18).max(120),
    website: Joi.string().uri()
});

app.post('/api/users', async (req, res) => {
    const { error, value } = userSchema.validate(req.body);
    
    if (error) {
        return res.status(400).json({
            errors: error.details.map(d => ({
                field: d.path.join('.'),
                message: d.message
            }))
        });
    }
    
    // value contient les données validées
    const user = await User.create(value);
    res.json(user);
});
```

**Sanitization:**

```javascript
const validator = require('validator');
const xss = require('xss');

// Échapper HTML
const cleanName = validator.escape(req.body.name);

// Supprimer tags HTML
const cleanContent = xss(req.body.content);

// Normaliser email
const email = validator.normalizeEmail(req.body.email);

// Validation URL
if (!validator.isURL(req.body.website)) {
    throw new Error('Invalid URL');
}
```

### 10.2.2 Gestion des sessions

**Sessions sécurisées:**

```javascript
const session = require('express-session');
const RedisStore = require('connect-redis')(session);
const redis = require('redis');

const redisClient = redis.createClient();

app.use(session({
    store: new RedisStore({ client: redisClient }),
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: false,
    cookie: {
        secure: true,        // HTTPS uniquement
        httpOnly: true,      // Pas d'accès JavaScript
        maxAge: 3600000,     // 1 heure
        sameSite: 'strict'   // Protection CSRF
    }
}));
```

**CSRF Protection:**

```javascript
const csrf = require('csurf');
const csrfProtection = csrf({ cookie: true });

app.get('/form', csrfProtection, (req, res) => {
    res.render('form', { csrfToken: req.csrfToken() });
});

app.post('/submit', csrfProtection, (req, res) => {
    // Protected against CSRF
});
```

---

## 10.3 Chiffrement et stockage sécurisé des données

### 10.3.1 Chiffrement symétrique

```javascript
const crypto = require('crypto');

// Chiffrer
function encrypt(text, key) {
    const iv = crypto.randomBytes(16);
    const cipher = crypto.createCipheriv('aes-256-cbc', Buffer.from(key), iv);
    
    let encrypted = cipher.update(text, 'utf8', 'hex');
    encrypted += cipher.final('hex');
    
    return iv.toString('hex') + ':' + encrypted;
}

// Déchiffrer
function decrypt(text, key) {
    const parts = text.split(':');
    const iv = Buffer.from(parts.shift(), 'hex');
    const encryptedText = Buffer.from(parts.join(':'), 'hex');
    
    const decipher = crypto.createDecipheriv('aes-256-cbc', Buffer.from(key), iv);
    
    let decrypted = decipher.update(encryptedText, 'hex', 'utf8');
    decrypted += decipher.final('utf8');
    
    return decrypted;
}
```

### 10.3.2 Chiffrement asymétrique (RSA)

```javascript
// Générer paire de clés
const { publicKey, privateKey } = crypto.generateKeyPairSync('rsa', {
    modulusLength: 2048,
    publicKeyEncoding: {
        type: 'spki',
        format: 'pem'
    },
    privateKeyEncoding: {
        type: 'pkcs8',
        format: 'pem'
    }
});

// Chiffrer avec clé publique
function encryptWithPublicKey(text, publicKey) {
    const buffer = Buffer.from(text, 'utf8');
    const encrypted = crypto.publicEncrypt(publicKey, buffer);
    return encrypted.toString('base64');
}

// Déchiffrer avec clé privée
function decryptWithPrivateKey(encrypted, privateKey) {
    const buffer = Buffer.from(encrypted, 'base64');
    const decrypted = crypto.privateDecrypt(privateKey, buffer);
    return decrypted.toString('utf8');
}
```

### 10.3.3 Hachage sécurisé

```javascript
// Pour mots de passe: bcrypt (lent volontairement)
const bcrypt = require('bcrypt');
const hash = await bcrypt.hash(password, 12);

// Pour intégrité: SHA-256
const hash = crypto.createHash('sha256').update(data).digest('hex');

// Pour signatures: HMAC
const hmac = crypto.createHmac('sha256', secret);
hmac.update(data);
const signature = hmac.digest('hex');
```

---

## 10.4 Audit de sécurité et pentests

### 10.4.1 Outils d'analyse statique (SAST)

```bash
# SonarQube
sonar-scanner

# ESLint avec plugins sécurité
npm install --save-dev eslint-plugin-security
```

```javascript
// .eslintrc.js
module.exports = {
    plugins: ['security'],
    extends: ['plugin:security/recommended'],
    rules: {
        'security/detect-object-injection': 'error',
        'security/detect-non-literal-regexp': 'error',
        'security/detect-unsafe-regex': 'error'
    }
};
```

### 10.4.2 Tests de pénétration automatisés

**OWASP ZAP (Zed Attack Proxy):**

```bash
# Scan rapide
zap-cli quick-scan --self-contained http://localhost:3000

# Scan complet
zap-cli active-scan http://localhost:3000
```

**Burp Suite:** Proxy pour tester manuellement les requêtes

### 10.4.3 Checklist d'audit

```markdown
## Authentication & Authorization
- [ ] Mots de passe hashés (bcrypt, Argon2)
- [ ] MFA disponible
- [ ] Sessions sécurisées (httpOnly, secure, sameSite)
- [ ] Tokens JWT avec expiration
- [ ] Rate limiting sur login
- [ ] Vérification des autorisations sur chaque endpoint

## Input Validation
- [ ] Validation côté serveur
- [ ] Sanitization des entrées
- [ ] Requêtes paramétrées (SQL)
- [ ] Protection XSS
- [ ] Protection CSRF

## Data Protection
- [ ] HTTPS obligatoire
- [ ] Données sensibles chiffrées at-rest
- [ ] Backup chiffrés
- [ ] Logs sans données sensibles
- [ ] Conformité RGPD

## Infrastructure
- [ ] Dépendances à jour
- [ ] Serveurs patchés
- [ ] Firewall configuré
- [ ] Ports non nécessaires fermés
- [ ] Logs centralisés et monitorés

## Error Handling
- [ ] Messages d'erreur génériques en production
- [ ] Stack traces désactivées
- [ ] Logging des erreurs sécurisé

## API Security
- [ ] Rate limiting
- [ ] CORS configuré correctement
- [ ] Validation des content-types
- [ ] API keys/tokens sécurisés
```

---

## 📝 Résumé

✅ **OWASP Top 10:** Connaître et prévenir les vulnérabilités majeures

✅ **Validation:** Côté serveur obligatoire, sanitization systématique

✅ **Sessions:** httpOnly, secure, sameSite, stockage Redis

✅ **Chiffrement:** AES-256 (symétrique), RSA (asymétrique), bcrypt (mots de passe)

✅ **Audit:** SAST, DAST, pentests réguliers

---

## 💡 Exercices

1. Identifier et corriger 5 vulnérabilités dans un code fourni
2. Implémenter authentification JWT + MFA avec Speakeasy
3. Réaliser un audit de sécurité avec OWASP ZAP

---

[⬅️ Partie IV](./README.md) | [➡️ Chapitre 11](./chapitre-11-optimisation-performance.md)

