# Chapitre 12 : Maintenance et support

> **Objectif:** Gérer efficacement les versions logicielles, les correctifs, la documentation et les incidents pour assurer la pérennité et la qualité du système en production.

---

## 📋 Table des matières

1. [Gestion des versions logicielles](#121-gestion-des-versions-logicielles)
2. [Correctifs, patches et évolutions continues](#122-correctifs-patches-et-évolutions-continues)
3. [Documentation technique et fonctionnelle](#123-documentation-technique-et-fonctionnelle)
4. [Gestion des incidents et amélioration continue](#124-gestion-des-incidents-et-amélioration-continue)

---

## 12.1 Gestion des versions logicielles

### 12.1.1 Semantic Versioning (SemVer)

```
Format: MAJOR.MINOR.PATCH

Exemple: 2.4.7

MAJOR (2): Breaking changes (incompatibilité avec versions précédentes)
MINOR (4): Nouvelles fonctionnalités (rétrocompatible)
PATCH (7): Corrections de bugs (rétrocompatible)

Exemples:
1.0.0 → 1.0.1  Bug fix
1.0.1 → 1.1.0  Nouvelle fonctionnalité
1.1.0 → 2.0.0  Breaking change
```

**Pré-releases:**

```
1.0.0-alpha.1  Première alpha
1.0.0-beta.2   Deuxième beta
1.0.0-rc.1     Release candidate
1.0.0          Version stable
```

### 12.1.2 Changelog

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]
### Added
- New feature in development

## [2.1.0] - 2024-10-20
### Added
- User profile customization
- Export data to CSV functionality

### Changed
- Improved search performance by 50%
- Updated UI design for better UX

### Fixed
- Fixed memory leak in background task
- Corrected timezone display issue

### Security
- Updated dependencies with security patches

## [2.0.0] - 2024-09-15
### Added
- Complete redesign of the API
- GraphQL support

### Changed
- **BREAKING**: Authentication now requires JWT tokens
- **BREAKING**: Removed support for API v1

### Removed
- Legacy XML response format
```

### 12.1.3 Release Management

**Git Flow release:**

```bash
# Créer branche release
git checkout -b release/2.1.0 develop

# Bumper version
npm version minor  # 2.0.x → 2.1.0

# Finaliser
git checkout main
git merge release/2.1.0
git tag -a v2.1.0 -m "Release version 2.1.0"

git checkout develop
git merge release/2.1.0

git branch -d release/2.1.0

# Push
git push origin main develop --tags
```

**Automatisation avec CI/CD:**

```yaml
# .github/workflows/release.yml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  build-and-release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build
        run: npm run build
      
      - name: Create Release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ github.ref }}
          release_name: Release ${{ github.ref }}
          body_path: CHANGELOG.md
          draft: false
          prerelease: false
      
      - name: Publish to NPM
        run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

---

## 12.2 Correctifs, patches et évolutions continues

### 12.2.1 Hotfix process

```bash
# Créer branche hotfix depuis main
git checkout -b hotfix/critical-bug main

# Fix le bug
git commit -m "fix: resolve critical security vulnerability"

# Bumper version patch
npm version patch  # 2.1.0 → 2.1.1

# Merger dans main et develop
git checkout main
git merge hotfix/critical-bug
git tag -a v2.1.1 -m "Hotfix 2.1.1"

git checkout develop
git merge hotfix/critical-bug

git branch -d hotfix/critical-bug

# Déployer immédiatement
git push origin main develop --tags
```

### 12.2.2 Stratégies de déploiement

**Blue-Green Deployment:**

```yaml
# kubernetes/deployment-blue.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-blue
  labels:
    app: myapp
    version: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
      version: blue
  template:
    metadata:
      labels:
        app: myapp
        version: blue
    spec:
      containers:
      - name: myapp
        image: myapp:2.1.0
```

```bash
# Déployer nouvelle version (green)
kubectl apply -f deployment-green.yml

# Tester green
# Si OK, basculer le service
kubectl patch service myapp -p '{"spec":{"selector":{"version":"green"}}}'

# Si problème, rollback instantané
kubectl patch service myapp -p '{"spec":{"selector":{"version":"blue"}}}'
```

**Canary Deployment:**

```yaml
# 90% trafic vers stable, 10% vers canary
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  selector:
    app: myapp
  ports:
  - port: 80

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-stable
spec:
  replicas: 9
  selector:
    matchLabels:
      app: myapp
      version: stable

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-canary
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
      version: canary
```

### 12.2.3 Feature Flags

```javascript
const unleash = require('unleash-client');

unleash.initialize({
    url: 'http://unleash.example.com/api/',
    appName: 'my-app',
    customHeaders: {
        Authorization: process.env.UNLEASH_API_KEY
    }
});

// Utilisation
app.get('/api/orders', async (req, res) => {
    if (unleash.isEnabled('new-order-algorithm', req.user)) {
        return handleOrdersNewWay(req, res);
    } else {
        return handleOrdersOldWay(req, res);
    }
});

// Gradual rollout
unleash.isEnabled('premium-features', {
    userId: user.id,
    properties: {
        environment: 'production',
        userSegment: user.isPremium ? 'premium' : 'free'
    }
});
```

---

## 12.3 Documentation technique et fonctionnelle

### 12.3.1 Documentation API (OpenAPI/Swagger)

```yaml
openapi: 3.0.0
info:
  title: E-commerce API
  description: API for managing products and orders
  version: 2.1.0
  contact:
    name: API Support
    email: api@example.com

servers:
  - url: https://api.example.com/v2
    description: Production server
  - url: https://staging-api.example.com/v2
    description: Staging server

paths:
  /products:
    get:
      summary: List all products
      tags:
        - Products
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ProductList'

components:
  schemas:
    Product:
      type: object
      required:
        - id
        - name
        - price
      properties:
        id:
          type: integer
          example: 123
        name:
          type: string
          example: "Laptop Dell XPS 15"
        price:
          type: number
          format: decimal
          example: 1299.99
```

### 12.3.2 Documentation utilisateur

**Structure type:**

```markdown
# Guide utilisateur - Application E-commerce

## Table des matières
1. Premiers pas
2. Gestion du compte
3. Recherche et achat de produits
4. Suivi des commandes
5. FAQ
6. Support

## 1. Premiers pas

### Inscription
1. Cliquez sur "S'inscrire" en haut à droite
2. Remplissez le formulaire:
   - Email (requis)
   - Mot de passe (minimum 12 caractères)
   - Nom complet
3. Validez votre email en cliquant sur le lien reçu
4. Vous êtes maintenant connecté !

[Capture d'écran: Formulaire d'inscription]

### Première connexion
...
```

### 12.3.3 Architecture Decision Records (ADR)

```markdown
# ADR-003: Utilisation de Redis pour le cache de session

## Statut
Accepté

## Contexte
L'application actuelle stocke les sessions en mémoire (express-session default).
Problèmes rencontrés:
- Perte des sessions à chaque redémarrage
- Impossible de scaler horizontalement
- Sessions non partagées entre instances

## Décision
Utiliser Redis comme store de sessions partagé entre toutes les instances.

## Conséquences

### Positives
- Sessions persistantes entre redémarrages
- Scaling horizontal possible
- Performance accrue (Redis in-memory)
- TTL automatique des sessions

### Négatives
- Dépendance externe supplémentaire
- Coût infrastructure (serveur Redis)
- Légère latence réseau

## Alternatives considérées
- PostgreSQL: Trop lent pour sessions
- MongoDB: Over-engineering pour des sessions simples
- Sticky sessions: Ne résout pas la persistence

## Implémentation
```javascript
const session = require('express-session');
const RedisStore = require('connect-redis')(session);
const redis = require('redis');

const redisClient = redis.createClient({
    host: process.env.REDIS_HOST,
    port: 6379
});

app.use(session({
    store: new RedisStore({ client: redisClient }),
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: false
}));
\```

## Date
2024-10-15

## Références
- https://redis.io/
- https://github.com/tj/connect-redis
```

---

## 12.4 Gestion des incidents et amélioration continue

### 12.4.1 Processus de gestion d'incident

```
1. DÉTECTION
   - Monitoring alerte
   - Rapport utilisateur
   - Log d'erreur

2. TRIAGE
   - Évaluer la sévérité (P0-P4)
   - Assigner un owner
   - Créer ticket incident

3. INVESTIGATION
   - Analyser les logs
   - Reproduire le problème
   - Identifier la cause racine

4. RÉSOLUTION
   - Implémenter le fix
   - Tester la solution
   - Déployer le correctif

5. POST-MORTEM
   - Documenter l'incident
   - Actions préventives
   - Mise à jour monitoring
```

**Niveaux de sévérité:**

```
P0 (Critical): Service down, perte de données
    - SLA: Résolution < 1 heure
    - Escalade: Immédiate
    - Communication: Toutes les 30 minutes

P1 (High): Fonctionnalité majeure cassée
    - SLA: Résolution < 4 heures
    - Escalade: Si non résolu en 2h
    - Communication: Toutes les 2 heures

P2 (Medium): Fonctionnalité mineure cassée
    - SLA: Résolution < 1 jour
    - Escalade: Si non résolu en 8h
    - Communication: Quotidienne

P3 (Low): Bug mineur, workaround existe
    - SLA: Résolution < 1 semaine
    - Pas d'escalade
    - Communication: Hebdomadaire

P4 (Trivial): Amélioration, suggestion
    - SLA: Backlog
    - Traité dans prochaine release
```

### 12.4.2 Post-mortem

**Template:**

```markdown
# Post-Mortem: Panne base de données - 2024-10-20

## Résumé
Le 20 octobre 2024 de 14:30 à 15:45 UTC, la base de données principale
a cessé de répondre, causant une indisponibilité totale du service.

## Impact
- Durée: 1h15
- Utilisateurs affectés: 100%
- Transactions perdues: ~50
- Revenue estimé perdu: 5'000 CHF

## Chronologie

**14:30** - Alerte Prometheus: DB connection timeout
**14:32** - Équipe OnCall notifiée
**14:35** - Investigation: Disk full sur serveur DB
**14:40** - Décision: Purger logs anciens + étendre volume
**15:00** - Volume étendu (100GB → 500GB)
**15:15** - Service restauré progressivement
**15:45** - Tous services opérationnels

## Cause racine
Croissance exponentielle des logs applicatifs non rotationnés.
Espace disque saturé en 3 jours au lieu de semaines prévues.

## Résolution
1. Purge immédiate logs > 7 jours
2. Extension volume de 100GB à 500GB
3. Redémarrage service DB

## Actions préventives

### Court terme (cette semaine)
- [ ] Implémenter rotation automatique logs (logrotate)
- [ ] Alerte proactive à 80% disk usage
- [ ] Documentation procédure urgence "disk full"

### Moyen terme (ce mois)
- [ ] Migrer logs vers service centralisé (ELK)
- [ ] Configurer alertes prédictives (ML sur croissance)
- [ ] Runbook automatisé pour incidents disk

### Long terme (ce trimestre)
- [ ] Architecture sans état côté DB (logs externalisés)
- [ ] Backup automatique avec rétention configurable
- [ ] Chaos engineering pour tester résilience

## Leçons apprises
✅ Monitoring actuel a bien détecté le problème
✅ Équipe a réagi rapidement
❌ Pas d'alerte préventive avant saturation
❌ Pas de procédure documentée pour ce cas
❌ Backup logs manquait

## Participants
- John Doe (OnCall Engineer)
- Jane Smith (Database Admin)
- Mike Johnson (SRE Lead)
```

### 12.4.3 Métriques de qualité

**DORA Metrics:**

```
1. Deployment Frequency
   Combien de fois on déploie en production
   Cible: Multiple fois par jour

2. Lead Time for Changes
   Temps entre commit et production
   Cible: < 1 heure

3. Change Failure Rate
   % de déploiements causant des incidents
   Cible: < 15%

4. Time to Restore Service
   Temps pour restaurer après incident
   Cible: < 1 heure
```

**Tracking:**

```javascript
// Prometheus metrics
const deploymentCounter = new promClient.Counter({
    name: 'deployments_total',
    help: 'Total deployments',
    labelNames: ['environment', 'status']
});

const leadTime = new promClient.Histogram({
    name: 'deployment_lead_time_seconds',
    help: 'Time from commit to production',
    buckets: [60, 300, 900, 1800, 3600, 7200]
});

// Après chaque déploiement
deploymentCounter.inc({ environment: 'production', status: 'success' });

const commitTime = new Date(process.env.COMMIT_TIMESTAMP);
const deployTime = new Date();
leadTime.observe((deployTime - commitTime) / 1000);
```

---

## 📝 Résumé

✅ **Versioning:** SemVer, changelog, git tags, releases automatisées

✅ **Déploiements:** Blue-green, canary, feature flags

✅ **Documentation:** API (Swagger), utilisateur, ADRs

✅ **Incidents:** Process structuré, post-mortems, actions préventives

✅ **Amélioration continue:** DORA metrics, retrospectives

---

## 💡 Exercices

1. Créer un changelog complet pour une application fictive
2. Implémenter blue-green deployment avec Docker
3. Rédiger un post-mortem pour un incident simulé

---

## 📚 Pour aller plus loin

- 📖 **"Site Reliability Engineering"** - Google
- 📖 **"The Phoenix Project"** - Gene Kim
- 🔗 **keepachangelog.com**: Standard changelog
- 🔗 **semver.org**: Semantic Versioning

---

[⬅️ Chapitre 11](./chapitre-11-optimisation-performance.md) | [➡️ Partie V](../partie-5-gestion-projet-leadership/README.md)

