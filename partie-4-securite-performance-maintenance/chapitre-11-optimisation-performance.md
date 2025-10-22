# Chapitre 11 : Optimisation et performance

> **Objectif:** Diagnostiquer les goulots d'étranglement, optimiser les performances, implémenter des stratégies de caching et concevoir des architectures scalables.

---

## 📋 Table des matières

1. [Profilage et diagnostic des performances](#111-profilage-et-diagnostic-des-performances)
2. [Caching et optimisation de la mémoire](#112-caching-et-optimisation-de-la-mémoire)
3. [Scalabilité horizontale et verticale](#113-scalabilité-horizontale-et-verticale)
4. [Observabilité et logs centralisés](#114-observabilité-et-logs-centralisés)

---

## 11.1 Profilage et diagnostic des performances

### 11.1.1 Métriques clés

```
- Response Time: Temps de réponse (latence)
- Throughput: Nombre de requêtes/seconde
- Error Rate: Taux d'erreurs
- CPU Usage: Utilisation processeur
- Memory Usage: Utilisation mémoire
- Database Query Time: Temps des requêtes DB
```

### 11.1.2 Outils de profilage

**Node.js:**

```javascript
// Profiler intégré
node --prof app.js
node --prof-process isolate-*.log > processed.txt

// Clinic.js
clinic doctor -- node app.js
clinic flame -- node app.js

// New Relic / DataDog pour production
```

**Java:**

```bash
# JProfiler, YourKit, VisualVM

# Activer JMX
java -Dcom.sun.management.jmxremote \
     -Dcom.sun.management.jmxremote.port=9010 \
     -Dcom.sun.management.jmxremote.authenticate=false \
     -jar app.jar
```

**Requêtes SQL lentes:**

```sql
-- PostgreSQL: activer log des requêtes lentes
ALTER DATABASE mydb SET log_min_duration_statement = 1000;  -- 1 seconde

-- Analyser une requête
EXPLAIN ANALYZE
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id;
```

### 11.1.3 Optimisations base de données

**Index:**

```sql
-- Avant: scan complet (lent)
SELECT * FROM users WHERE email = 'john@example.com';  -- Seq Scan

-- Créer index
CREATE INDEX idx_users_email ON users(email);

-- Après: index scan (rapide)
SELECT * FROM users WHERE email = 'john@example.com';  -- Index Scan

-- Index composé
CREATE INDEX idx_orders_user_status ON orders(user_id, status);

-- Index partiel
CREATE INDEX idx_active_orders ON orders(user_id) WHERE status = 'ACTIVE';
```

**Requêtes N+1:**

```javascript
// ❌ N+1 Problem
const users = await User.findAll();
for (const user of users) {
    user.orders = await Order.findAll({ where: { userId: user.id } });
    // N requêtes supplémentaires !
}

// ✅ Solution: eager loading
const users = await User.findAll({
    include: [{ model: Order, as: 'orders' }]
});  // 1 seule requête avec JOIN
```

**Connection pooling:**

```javascript
// PostgreSQL avec node-postgres
const { Pool } = require('pg');

const pool = new Pool({
    host: 'localhost',
    database: 'mydb',
    max: 20,  // 20 connexions max
    idleTimeoutMillis: 30000,
    connectionTimeoutMillis: 2000,
});

// Utilisation
const client = await pool.connect();
try {
    const result = await client.query('SELECT * FROM users');
    return result.rows;
} finally {
    client.release();
}
```

---

## 11.2 Caching et optimisation de la mémoire

### 11.2.1 Stratégies de cache

**Cache-Aside (Lazy Loading):**

```javascript
async function getUser(userId) {
    const cacheKey = `user:${userId}`;
    
    // 1. Essayer le cache
    let user = await redis.get(cacheKey);
    if (user) {
        return JSON.parse(user);
    }
    
    // 2. Cache miss - charger de la DB
    user = await db.users.findByPk(userId);
    
    // 3. Mettre en cache
    await redis.setex(cacheKey, 3600, JSON.stringify(user));
    
    return user;
}
```

**Write-Through:**

```javascript
async function updateUser(userId, data) {
    // 1. Mettre à jour la DB
    const user = await db.users.update(data, { where: { id: userId } });
    
    // 2. Mettre à jour le cache immédiatement
    const cacheKey = `user:${userId}`;
    await redis.setex(cacheKey, 3600, JSON.stringify(user));
    
    return user;
}
```

**Write-Behind (Write-Back):**

```javascript
async function updateUser(userId, data) {
    // 1. Mettre à jour le cache
    const cacheKey = `user:${userId}`;
    await redis.setex(cacheKey, 3600, JSON.stringify(data));
    
    // 2. Ajouter à une queue pour mise à jour DB asynchrone
    await queue.add('update-user', { userId, data });
    
    return data;
}
```

### 11.2.2 Cache HTTP

**Headers de cache:**

```javascript
app.get('/api/products/:id', async (req, res) => {
    const product = await Product.findByPk(req.params.id);
    
    // Cache-Control
    res.set('Cache-Control', 'public, max-age=3600');  // 1 heure
    
    // ETag
    const etag = crypto.createHash('md5').update(JSON.stringify(product)).digest('hex');
    res.set('ETag', etag);
    
    // Si client a déjà la bonne version
    if (req.headers['if-none-match'] === etag) {
        return res.status(304).send();  // Not Modified
    }
    
    res.json(product);
});
```

**CDN pour assets statiques:**

```javascript
// Utiliser CloudFlare, AWS CloudFront, etc.
// Headers pour assets
res.set('Cache-Control', 'public, max-age=31536000, immutable');  // 1 an
```

### 11.2.3 Optimisation mémoire

**Memory leaks:**

```javascript
// ❌ Memory leak - listeners non nettoyés
function registerUser(user) {
    eventEmitter.on('order-created', (order) => {
        sendEmail(user.email, order);
    });
}

// ✅ Cleanup
function registerUser(user) {
    const handler = (order) => sendEmail(user.email, order);
    eventEmitter.on('order-created', handler);
    
    // Cleanup quand l'utilisateur se déconnecte
    user.onLogout(() => {
        eventEmitter.off('order-created', handler);
    });
}

// ❌ Memory leak - closure avec grande variable
function processData() {
    const largeArray = new Array(1000000).fill('data');
    
    return function() {
        console.log(largeArray[0]);  // Garde largeArray en mémoire !
    };
}

// ✅ Libérer la mémoire
function processData() {
    const largeArray = new Array(1000000).fill('data');
    const firstElement = largeArray[0];
    
    return function() {
        console.log(firstElement);  // largeArray peut être garbage collected
    };
}
```

**Object pooling:**

```javascript
class ConnectionPool {
    constructor(maxSize) {
        this.pool = [];
        this.maxSize = maxSize;
        this.active = 0;
    }
    
    async acquire() {
        if (this.pool.length > 0) {
            return this.pool.pop();
        }
        
        if (this.active < this.maxSize) {
            this.active++;
            return this.createConnection();
        }
        
        // Attendre qu'une connexion se libère
        await this.wait();
        return this.acquire();
    }
    
    release(connection) {
        this.pool.push(connection);
    }
}
```

---

## 11.3 Scalabilité horizontale et verticale

### 11.3.1 Scaling vertical

```
Augmenter les ressources d'un serveur unique:
- CPU: 4 cores → 16 cores
- RAM: 8 GB → 64 GB
- Disk: HDD → SSD NVMe

✅ Avantages:
  - Simple à implémenter
  - Pas de complexité distribuée

❌ Inconvénients:
  - Limite physique
  - Single point of failure
  - Coûteux au-delà d'un certain point
```

### 11.3.2 Scaling horizontal

```
Ajouter plus de serveurs:

           Load Balancer
                 │
     ┌───────────┼───────────┐
     │           │           │
  Server 1    Server 2    Server 3
     │           │           │
     └───────────┼───────────┘
                 │
             Database
```

**Load Balancing (NGINX):**

```nginx
upstream backend {
    least_conn;  # ou: ip_hash, round_robin
    
    server backend1.example.com:3000;
    server backend2.example.com:3000;
    server backend3.example.com:3000;
}

server {
    listen 80;
    
    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

**Session sticky avec Redis:**

```javascript
// Toutes les instances partagent le même Redis
const session = require('express-session');
const RedisStore = require('connect-redis')(session);

app.use(session({
    store: new RedisStore({ client: redisClient }),
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: false
}));
```

### 11.3.3 Database scaling

**Read replicas:**

```
         Master (Write)
              │
         Replication
              │
      ┌───────┴───────┐
      │               │
 Replica 1       Replica 2
   (Read)          (Read)
```

```javascript
const { Sequelize } = require('sequelize');

const sequelize = new Sequelize({
    replication: {
        read: [
            { host: 'replica1.example.com', username: 'reader', password: 'pass' },
            { host: 'replica2.example.com', username: 'reader', password: 'pass' }
        ],
        write: { host: 'master.example.com', username: 'writer', password: 'pass' }
    },
    pool: { max: 20, min: 5 }
});
```

**Sharding (partitionnement horizontal):**

```javascript
// Partitionner par user_id
function getUserShard(userId) {
    return userId % 4;  // 4 shards
}

const shard = getUserShard(userId);
const db = databases[shard];
const user = await db.users.findByPk(userId);
```

---

## 11.4 Observabilité et logs centralisés

### 11.4.1 Les 3 piliers de l'observabilité

**1. Logs**

```javascript
const winston = require('winston');

const logger = winston.createLogger({
    level: 'info',
    format: winston.format.json(),
    defaultMeta: { service: 'api', environment: process.env.NODE_ENV },
    transports: [
        new winston.transports.File({ filename: 'error.log', level: 'error' }),
        new winston.transports.File({ filename: 'combined.log' })
    ]
});

// Logs structurés
logger.info('User logged in', {
    userId: user.id,
    email: user.email,
    ip: req.ip,
    timestamp: new Date()
});

logger.error('Database connection failed', {
    error: err.message,
    stack: err.stack,
    database: 'users'
});
```

**2. Metrics**

```javascript
const promClient = require('prom-client');

// Counter
const httpRequestsTotal = new promClient.Counter({
    name: 'http_requests_total',
    help: 'Total HTTP requests',
    labelNames: ['method', 'route', 'status']
});

// Histogram
const httpRequestDuration = new promClient.Histogram({
    name: 'http_request_duration_seconds',
    help: 'HTTP request duration',
    labelNames: ['method', 'route', 'status']
});

// Middleware
app.use((req, res, next) => {
    const start = Date.now();
    
    res.on('finish', () => {
        const duration = (Date.now() - start) / 1000;
        
        httpRequestsTotal.inc({
            method: req.method,
            route: req.route?.path || req.path,
            status: res.statusCode
        });
        
        httpRequestDuration.observe({
            method: req.method,
            route: req.route?.path || req.path,
            status: res.statusCode
        }, duration);
    });
    
    next();
});

// Endpoint pour Prometheus
app.get('/metrics', async (req, res) => {
    res.set('Content-Type', promClient.register.contentType);
    res.end(await promClient.register.metrics());
});
```

**3. Traces (Distributed Tracing)**

```javascript
const opentelemetry = require('@opentelemetry/api');
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');

const provider = new NodeTracerProvider();
provider.register();

const tracer = opentelemetry.trace.getTracer('my-service');

app.get('/api/users/:id', async (req, res) => {
    const span = tracer.startSpan('get-user');
    
    try {
        span.setAttribute('user.id', req.params.id);
        
        const user = await getUser(req.params.id);
        span.addEvent('user-retrieved');
        
        res.json(user);
    } catch (error) {
        span.recordException(error);
        span.setStatus({ code: opentelemetry.SpanStatusCode.ERROR });
        throw error;
    } finally {
        span.end();
    }
});
```

### 11.4.2 Stack ELK (Elasticsearch, Logstash, Kibana)

```yaml
# docker-compose.yml
version: '3'
services:
  elasticsearch:
    image: elasticsearch:8.10.0
    environment:
      - discovery.type=single-node
    ports:
      - "9200:9200"
  
  logstash:
    image: logstash:8.10.0
    volumes:
      - ./logstash.conf:/usr/share/logstash/pipeline/logstash.conf
    depends_on:
      - elasticsearch
  
  kibana:
    image: kibana:8.10.0
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch
```

### 11.4.3 Alerting

**Prometheus Alertmanager:**

```yaml
# alerts.yml
groups:
  - name: example
    rules:
      - alert: HighErrorRate
        expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.05
        for: 10m
        labels:
          severity: critical
        annotations:
          summary: "High error rate detected"
          description: "Error rate is {{ $value }} for {{ $labels.instance }}"
      
      - alert: HighLatency
        expr: histogram_quantile(0.95, http_request_duration_seconds) > 1
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High latency detected"
```

---

## 📝 Résumé

✅ **Profilage:** Identifier goulots via profilers, EXPLAIN ANALYZE

✅ **Optimisation DB:** Index, eager loading, connection pooling

✅ **Caching:** Redis, HTTP cache, CDN pour performances

✅ **Scaling:** Horizontal (load balancing) vs Vertical (plus de ressources)

✅ **Observabilité:** Logs structurés, métriques Prometheus, traces distribuées

---

## 💡 Exercices

1. Optimiser une API lente (identifier avec profiler, ajouter index, caching)
2. Implémenter load balancing avec NGINX + 3 instances Node.js
3. Mettre en place monitoring avec Prometheus + Grafana

---

[⬅️ Chapitre 10](./chapitre-10-securite-logicielle.md) | [➡️ Chapitre 12](./chapitre-12-maintenance-support.md)

