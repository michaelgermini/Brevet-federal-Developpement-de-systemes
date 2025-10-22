# 🚀 Cheat Sheet - Brevet fédéral Développement de systèmes

> **Raccourcis et mémos pour la révision rapide**

---

## 📋 Principes SOLID

| Principe | Règle | Exemple |
|----------|-------|---------|
| **S**RP | Une classe = une responsabilité | `UserService` gère utilisateurs, pas emails |
| **O**CP | Extension sans modification | Patterns Strategy/Factory |
| **L**SP | Sous-types interchangeables | Square hérite de Rectangle correctement |
| **I**SP | Interfaces spécifiques | `Worker` → `Workable` + `Eatable` |
| **D**IP | Dépendre d'abstractions | `OrderService` utilise `IOrderRepository` |

---

## 🔄 Modèles d'architecture

### Monolithique
```
✅ Simple, performant (pas de réseau)
❌ Scaling difficile, déploiement risqué
```

### Microservices
```
✅ Scaling indépendant, technologies diverses
❌ Complexité opérationnelle, transactions distribuées
```

### SOA
```
✅ Réutilisation, interopérabilité
❌ Overhead XML, couplage ESB
```

---

## 🧪 Pyramide des tests

```
E2E Tests (10%)     ← Lent, fragile, coûteux
    │
Integration Tests (20%)  ← Tests composants ensemble
    │
Unit Tests (70%)    ← Rapide, isolé, fiable
```

---

## 🔐 OWASP Top 10

| # | Vulnérabilité | Protection |
|---|---------------|------------|
| A01 | Broken Access Control | Vérifier autorisations côté serveur |
| A02 | Cryptographic Failures | Chiffrement AES-256, HTTPS |
| A03 | Injection | Requêtes paramétrées, validation |
| A04 | Insecure Design | Threat Modeling, Security by Design |
| A05 | Security Misconfiguration | Configuration hardening |
| A06 | Vulnerable Components | Audit dépendances, mises à jour |
| A07 | ID/Auth Failures | MFA, rate limiting, sessions sécurisées |
| A08 | Software Integrity | Signature, vérification intégrité |
| A09 | Security Logging | Logs détaillés, monitoring |
| A10 | SSRF | Whitelist domaines, validation URLs |

---

## 🎯 Patterns de conception

### Créationnels
- **Factory**: `createUser()` selon type
- **Singleton**: `Database.getInstance()`
- **Builder**: `User.builder().name("John").email("...").build()`

### Structurels
- **Adapter**: Interface compatible (Stripe → PaymentProcessor)
- **Decorator**: Fonctionnalités dynamiques (MilkDecorator)
- **Facade**: Interface simplifiée (API Gateway)

### Comportementaux
- **Observer**: Notification événements
- **Strategy**: Algorithmes interchangeables
- **Command**: Encapsulation actions (Undo/Redo)

---

## 🗃️ SQL Essentials

### Jointures
```sql
-- INNER JOIN: intersection
SELECT * FROM users u INNER JOIN orders o ON u.id = o.user_id

-- LEFT JOIN: tous users + orders si existant
SELECT * FROM users u LEFT JOIN orders o ON u.id = o.user_id

-- RIGHT JOIN: toutes orders + users si existant
SELECT * FROM orders o RIGHT JOIN users u ON o.user_id = u.id
```

### Index
```sql
-- Index simple
CREATE INDEX idx_users_email ON users(email);

-- Index composite
CREATE INDEX idx_orders_user_status ON orders(user_id, status);

-- Index partiel
CREATE INDEX idx_active_orders ON orders(user_id) WHERE status = 'ACTIVE';
```

### Transactions
```sql
BEGIN;
  UPDATE products SET stock = stock - 1 WHERE id = 101;
  INSERT INTO orders (user_id, product_id) VALUES (1, 101);
COMMIT;  -- Ou ROLLBACK en cas d'erreur
```

---

## 🌐 REST API

### Méthodes HTTP
```
GET    /users      → Liste utilisateurs
GET    /users/123  → Utilisateur 123
POST   /users      → Créer utilisateur
PUT    /users/123  → Remplacer utilisateur 123
PATCH  /users/123  → Modifier partiellement
DELETE /users/123  → Supprimer utilisateur
```

### Codes de statut
```
2xx Success: 200 OK, 201 Created, 204 No Content
3xx Redirect: 301 Moved Permanently, 304 Not Modified
4xx Client: 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found
5xx Server: 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable
```

### Bonnes pratiques
- Utiliser noms au pluriel
- Versioning: `/api/v1/users`
- Pagination: `?page=1&limit=20`
- Filtrage: `?status=active&sort=-created_at`
- HATEOAS: liens dans responses

---

## 🔒 Sécurité

### JWT Structure
```
Header.Payload.Signature

Header:  {"alg": "HS256", "typ": "JWT"}
Payload: {"sub": "123", "email": "user@ex.com", "exp": 1638360000}
Signature: HMACSHA256(base64(header) + "." + base64(payload), secret)
```

### Headers de sécurité
```
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Strict-Transport-Security: max-age=31536000
Content-Security-Policy: default-src 'self'
```

### Validation entrées
```javascript
// Joi schema
const schema = Joi.object({
  email: Joi.string().email().required(),
  password: Joi.string().min(12).required(),
  age: Joi.number().integer().min(18).max(120)
});

// Validation
const { error, value } = schema.validate(req.body);
```

---

## 🐳 Docker

### Commands essentiels
```bash
# Build image
docker build -t myapp:1.0 .

# Run container
docker run -d -p 3000:3000 --name myapp myapp:1.0

# Logs
docker logs myapp

# Exec dans container
docker exec -it myapp bash

# Stop/remove
docker stop myapp
docker rm myapp

# Docker Compose
docker-compose up -d
docker-compose down
```

### Dockerfile optimisé
```dockerfile
# Multi-stage build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:18-alpine AS production
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001

WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY --chown=nextjs:nodejs . .

USER nextjs
EXPOSE 3000
CMD ["npm", "start"]
```

---

## ☸️ Kubernetes

### Concepts clés
- **Pod**: Plus petite unité déployable
- **Service**: Exposition pods (ClusterIP, NodePort, LoadBalancer)
- **Deployment**: Gestion réplicas pods
- **ConfigMap/Secret**: Configuration/sensibles
- **Ingress**: Routage HTTP externe

### Commands kubectl
```bash
# Lister ressources
kubectl get pods
kubectl get services
kubectl get deployments

# Logs pod
kubectl logs pod-name

# Exec dans pod
kubectl exec -it pod-name -- bash

# Appliquer manifests
kubectl apply -f deployment.yaml

# Déboguer
kubectl describe pod pod-name
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## 📊 Monitoring

### Métriques clés
- **Response Time**: < 500ms (95th percentile)
- **Throughput**: Requêtes/seconde
- **Error Rate**: < 1%
- **Availability**: > 99.9%
- **CPU/Memory**: < 80%

### Prometheus queries
```promql
# Taux d'erreur HTTP
rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m]) * 100

# Latence 95th percentile
histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[10m]))

# Utilisation CPU par pod
rate(container_cpu_usage_seconds_total{pod=~"$pod"}[5m]) * 100
```

---

## 🧪 Tests

### JUnit 5
```java
@Test
@DisplayName("Should create user")
void shouldCreateUser() {
    // Arrange
    UserService service = new UserService(mockRepo);
    CreateUserRequest req = new CreateUserRequest("john@example.com", "password");

    // Act
    User user = service.createUser(req);

    // Assert
    assertNotNull(user);
    assertEquals("john@example.com", user.getEmail());
    verify(mockRepo).save(any(User.class));
}
```

### Jest (JavaScript)
```javascript
describe('UserService', () => {
  it('should create user', async () => {
    const mockRepo = { save: jest.fn() };
    const service = new UserService(mockRepo);

    const user = await service.createUser({
      email: 'john@example.com',
      password: 'password123'
    });

    expect(user.email).toBe('john@example.com');
    expect(mockRepo.save).toHaveBeenCalled();
  });
});
```

---

## 🔀 Git

### Workflows courants
```bash
# Git Flow
git checkout develop
git checkout -b feature/new-feature
# ... développement ...
git checkout develop
git merge feature/new-feature

# GitHub Flow
git checkout -b feature/login
# ... développement ...
# Create PR, review, merge
git checkout main
git pull origin main
git branch -d feature/login

# Résoudre conflits
git fetch origin
git merge origin/main
# Éditer fichiers en conflit
git add <resolved-files>
git commit
```

### Conventional Commits
```
feat: add user authentication
fix: resolve null pointer exception
docs: update API documentation
refactor: extract validation logic
test: add unit tests for UserService
chore: update dependencies
```

---

## 📈 Performance

### Optimisations database
```sql
-- EXPLAIN plan
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';

-- Index manquant?
CREATE INDEX CONCURRENTLY idx_users_email ON users(email);

-- Query lente?
SELECT COUNT(*) FROM orders WHERE created_at > '2024-01-01';
-- → Utiliser index sur created_at
```

### Cache strategies
```javascript
// Cache-Aside
const user = await cache.get(`user:${id}`);
if (!user) {
  user = await db.getUser(id);
  await cache.setex(`user:${id}`, 3600, JSON.stringify(user));
}
```

### Profiling Node.js
```bash
# CPU profiling
node --prof app.js
node --prof-process isolate-*.log > report.txt

# Memory profiling
node --inspect app.js
# Ouvrir chrome://inspect
```

---

## 🏗️ Architecture Decision Records (ADR)

Template rapide:
```markdown
# ADR-XXX: [Titre]

## Status: [Proposed | Accepted | Deprecated]

## Context
[Pourquoi cette décision?]

## Decision
[Qu'avons-nous décidé?]

## Consequences
[Impacts positifs/négatifs, risques, mitigation]
```

---

## 🎯 Checklist projet Brevet fédéral

### Avant démarrage
- [ ] Sujet validé avec expert
- [ ] Cahier des charges complet
- [ ] Technologies choisies (justifiées)
- [ ] Planning réaliste (5-6 mois)

### Développement
- [ ] Tests automatisés (>80% coverage)
- [ ] Code review systématique
- [ ] Documentation technique
- [ ] Sécurité (OWASP Top 10)
- [ ] Performance optimisée

### Livrables
- [ ] Rapport technique (50+ pages)
- [ ] Application fonctionnelle
- [ ] Code source (GitHub)
- [ ] Diagrammes UML
- [ ] Tests passant
- [ ] Démo préparée

### Soutenance
- [ ] Slides clairs (20 min)
- [ ] Démo live (fonctionnelle)
- [ ] Q&A préparées
- [ ] Stress management

---

## 📚 Ressources rapides

### Docs officielles
- [MDN Web Docs](https://developer.mozilla.org) - Web
- [OWASP Top 10](https://owasp.org/www-project-top-ten) - Sécurité
- [ISO 25010](https://iso.org) - Qualité logicielle

### Outils
- [GitHub](https://github.com) - Code + CI/CD
- [Postman](https://postman.com) - API testing
- [Docker Hub](https://hub.docker.com) - Images

### Communautés
- [Stack Overflow](https://stackoverflow.com) - Q&A
- [Reddit r/programming](https://reddit.com/r/programming) - Discussion
- [Dev.to](https://dev.to) - Articles

---

*Ce cheat sheet est votre compagnon de révision. Utilisez-le pour rafraîchir rapidement vos connaissances avant l'examen ou un entretien !*

---

[⬅️ Retour à l'accueil](./README.md) | [🔝 Haut de page](#-cheat-sheet---brevet-fédéral-développement-de-systèmes)

