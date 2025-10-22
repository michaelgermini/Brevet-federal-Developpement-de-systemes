# Chapitre 5 : Développement collaboratif

> **Objectif:** Maîtriser la gestion de versions avec Git, l'intégration continue (CI/CD), la collaboration en équipe et les bonnes pratiques de revue de code.

---

## 📋 Table des matières

1. [Gestion de versions (Git, GitHub, GitLab, workflows)](#51-gestion-de-versions-git-github-gitlab-workflows)
2. [Intégration continue (CI/CD) et automatisation des tests](#52-intégration-continue-cicd-et-automatisation-des-tests)
3. [Collaboration entre équipes (frontend, backend, DevOps)](#53-collaboration-entre-équipes-frontend-backend-devops)
4. [Bonnes pratiques de revue de code et documentation](#54-bonnes-pratiques-de-revue-de-code-et-documentation)

---

## 5.1 Gestion de versions (Git, GitHub, GitLab, workflows)

### 5.1.1 Fondamentaux de Git

**Concepts clés:**

```bash
# Configuration initiale
git config --global user.name "John Doe"
git config --global user.email "john@example.com"

# Initialiser un dépôt
git init
git clone https://github.com/user/repo.git

# Zone de staging
git add fichier.txt          # Ajouter un fichier
git add .                    # Ajouter tous les changements
git reset fichier.txt        # Retirer du staging

# Commits
git commit -m "feat: add user authentication"
git commit --amend           # Modifier le dernier commit

# Historique
git log                      # Historique complet
git log --oneline            # Historique condensé
git log --graph --all        # Visualisation graphique

# Branches
git branch feature/login     # Créer une branche
git checkout feature/login   # Changer de branche
git checkout -b feature/api  # Créer et changer
git branch -d feature/login  # Supprimer une branche

# Fusion
git merge feature/login      # Fusionner une branche
git rebase main             # Rebaser sur main

# Remote
git remote add origin https://github.com/user/repo.git
git push origin main         # Pousser vers le serveur
git pull origin main         # Récupérer et fusionner
git fetch origin             # Récupérer sans fusionner
```

**États des fichiers Git:**

```
Working Directory → Staging Area → Local Repository → Remote Repository
   (modifié)         (git add)      (git commit)       (git push)

┌─────────────┐   git add    ┌─────────────┐   git commit  ┌─────────────┐
│   Untracked │────────────→ │   Staged    │─────────────→ │  Committed  │
│  Modified   │              │             │               │             │
└─────────────┘              └─────────────┘               └─────────────┘
       ↑                            │                              │
       │                    git reset                          git push
       │                            │                              │
       └────────────────────────────┘                              ▼
                                                           ┌─────────────┐
                                                           │   Remote    │
                                                           └─────────────┘
```

### 5.1.2 Conventional Commits

**Format standardisé pour les messages de commit:**

```
<type>(<scope>): <description>

[corps optionnel]

[pied(s) optionnel(s)]
```

**Types:**
- `feat`: Nouvelle fonctionnalité
- `fix`: Correction de bug
- `docs`: Documentation uniquement
- `style`: Formatage, point-virgules manquants, etc.
- `refactor`: Refactoring sans changement fonctionnel
- `perf`: Amélioration de performance
- `test`: Ajout ou correction de tests
- `chore`: Tâches de maintenance (build, outils, etc.)

**Exemples:**

```bash
# Feature
git commit -m "feat(auth): add JWT authentication"

# Bug fix
git commit -m "fix(api): resolve null pointer exception in user service"

# Documentation
git commit -m "docs(readme): update installation instructions"

# Breaking change
git commit -m "feat(api)!: change endpoint structure

BREAKING CHANGE: /api/users endpoint now returns paginated results"

# With scope and description
git commit -m "refactor(order): extract payment processing to separate service

- Create PaymentService class
- Move payment logic from OrderService
- Add unit tests for PaymentService"
```

### 5.1.3 Git Workflows

#### Git Flow

**Branches principales:**
- `main` (ou `master`): Code en production
- `develop`: Intégration des fonctionnalités

**Branches de support:**
- `feature/*`: Nouvelles fonctionnalités
- `release/*`: Préparation d'une release
- `hotfix/*`: Corrections urgentes en production

```
main      ──●────────────────●──────●──────●────→
             │                ↑      ↑      ↑
             │             release hotfix hotfix
             │                │      │      │
develop   ───●───●───●───●───●──●───┘      │
             │   │   │   │   │  │          │
             │   │   │   │   │  └─release/*│
             │   │   │   │   │             │
feature/* ───┘   │   │   │   └─feature-3   │
                 │   │   │                  │
             feature-1 │   └─feature-2      │
                       │                    │
                   feature-1.1               │
                                             │
hotfix/*  ───────────────────────────────────┘
```

**Workflow:**

```bash
# Nouvelle fonctionnalité
git checkout develop
git checkout -b feature/user-profile
# ... développement ...
git commit -m "feat: add user profile page"
git checkout develop
git merge feature/user-profile
git branch -d feature/user-profile

# Préparer une release
git checkout develop
git checkout -b release/1.2.0
# ... corrections mineures, mise à jour version ...
git commit -m "chore: bump version to 1.2.0"
git checkout main
git merge release/1.2.0
git tag -a v1.2.0 -m "Release 1.2.0"
git checkout develop
git merge release/1.2.0
git branch -d release/1.2.0

# Hotfix urgent
git checkout main
git checkout -b hotfix/critical-bug
# ... correction ...
git commit -m "fix: resolve critical security vulnerability"
git checkout main
git merge hotfix/critical-bug
git tag -a v1.2.1 -m "Hotfix 1.2.1"
git checkout develop
git merge hotfix/critical-bug
git branch -d hotfix/critical-bug
```

#### GitHub Flow (simplifié)

**Branches:**
- `main`: Toujours déployable
- `feature/*`: Branches de fonctionnalités

```
main  ──●─────●────────●────────●───────→
        │     ↑        ↑        ↑
        │     │        │        │
        │   merge    merge    merge
        │     │        │        │
feat-1  └──●──●──●─────┘        │
                │               │
feat-2  ────────└──●──●──●──────┘
                       │
feat-3  ───────────────└──●──●──●
```

**Workflow:**

```bash
# 1. Créer une branche
git checkout -b feature/new-api

# 2. Développer et commiter régulièrement
git commit -m "feat: add endpoint for user search"

# 3. Pusher et créer une Pull Request
git push origin feature/new-api
# Créer PR sur GitHub

# 4. Revue de code et discussion
# Corrections basées sur les commentaires

# 5. Merge après approbation
# Via l'interface GitHub (squash ou merge)

# 6. Supprimer la branche
git branch -d feature/new-api
git push origin --delete feature/new-api
```

#### Trunk-Based Development

**Principe:** Développement sur `main` avec branches de courte durée (< 1 jour).

```
main  ──●─●─●─●─●─●─●─●─●─●─●─●─●─●───→
        │ ↑ │ ↑ │ ↑ │ ↑ │ ↑ │ ↑ │
        │ │ │ │ │ │ │ │ │ │ │ │ │
        └─┘ └─┘ └─┘ └─┘ └─┘ └─┘ └─┘
       Short-lived branches (< 24h)
```

**Avantages:**
- Intégration continue vraie
- Moins de conflits de merge
- Feedback rapide
- CI/CD optimal

**Pratiques:**
- Feature flags pour fonctionnalités incomplètes
- Commits fréquents
- Tests automatiques robustes
- Revues de code rapides

### 5.1.4 Résolution de conflits

**Scénario de conflit:**

```bash
# Votre branche
git checkout feature/my-feature
# ... modifications du fichier app.js ...
git commit -m "feat: add new feature"

# Pendant ce temps, main a été modifié
git checkout main
git pull origin main

# Tentative de merge
git checkout feature/my-feature
git merge main

# CONFLICT!
Auto-merging app.js
CONFLICT (content): Merge conflict in app.js
Automatic merge failed; fix conflicts and then commit the result.
```

**Fichier avec conflit:**

```javascript
function calculateTotal(items) {
<<<<<<< HEAD (votre version)
    return items.reduce((sum, item) => sum + item.price * item.quantity, 0);
=======
    return items.map(item => item.price * item.qty)
                .reduce((a, b) => a + b, 0);
>>>>>>> main (version de main)
}
```

**Résolution:**

```bash
# 1. Ouvrir le fichier et choisir la version
# ou combiner les deux

# 2. Version finale (après édition manuelle)
function calculateTotal(items) {
    return items.reduce((sum, item) => 
        sum + (item.price * (item.quantity || item.qty)), 0);
}

# 3. Marquer comme résolu
git add app.js

# 4. Finaliser le merge
git commit -m "merge: resolve conflict in calculateTotal"

# Outils pour aider
git mergetool           # Ouvre un outil visuel
git checkout --ours app.js    # Garder notre version
git checkout --theirs app.js  # Garder leur version
```

### 5.1.5 Git avancé

**Stash (mise de côté temporaire):**

```bash
# Sauvegarder les modifications en cours
git stash

# Lister les stash
git stash list

# Réappliquer le dernier stash
git stash pop

# Réappliquer un stash spécifique
git stash apply stash@{1}

# Créer un stash avec message
git stash save "WIP: refactoring user service"
```

**Cherry-pick (appliquer un commit spécifique):**

```bash
# Appliquer le commit abc123 sur la branche courante
git cherry-pick abc123

# Appliquer plusieurs commits
git cherry-pick abc123 def456 ghi789
```

**Rebase interactif (réécrire l'historique):**

```bash
# Modifier les 3 derniers commits
git rebase -i HEAD~3

# Éditeur s'ouvre:
pick abc123 feat: add login
pick def456 fix: typo
pick ghi789 feat: add logout

# Modifier en:
pick abc123 feat: add login
squash def456 fix: typo    # Fusionner avec le précédent
reword ghi789 feat: add logout  # Modifier le message
```

**Bisect (recherche de bug):**

```bash
# Démarrer la recherche binaire
git bisect start
git bisect bad                  # Commit actuel est mauvais
git bisect good v1.0.0          # v1.0.0 était bon

# Git checkout un commit au milieu
# Tester et marquer:
git bisect good  # ou bad

# Répéter jusqu'à trouver le commit problématique
# Terminer
git bisect reset
```

---

## 5.2 Intégration continue (CI/CD) et automatisation des tests

### 5.2.1 Qu'est-ce que CI/CD?

**CI (Continuous Integration):**
- Fusion fréquente du code dans la branche principale
- Build automatique à chaque commit
- Tests automatiques
- Feedback rapide

**CD (Continuous Delivery/Deployment):**
- **Continuous Delivery:** Prêt à déployer à tout moment
- **Continuous Deployment:** Déploiement automatique en production

```
┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
│  Commit  │──→│  Build   │──→│   Test   │──→│  Deploy  │──→│Production│
│   Code   │   │          │   │          │   │  Staging │   │          │
└──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘
     ↓             ↓              ↓               ↓               ↓
  Trigger      Compile        Unit Tests      Acceptance      Go Live
               Package        Integration    Manual/Auto
                             E2E Tests
```

### 5.2.2 GitHub Actions

**Exemple: Pipeline CI pour application Node.js**

`.github/workflows/ci.yml`:

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [16.x, 18.x, 20.x]
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v3
    
    - name: Setup Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run linter
      run: npm run lint
    
    - name: Run tests
      run: npm test
    
    - name: Upload coverage
      uses: codecov/codecov-action@v3
      with:
        files: ./coverage/coverage-final.json
  
  build:
    needs: test
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Build application
      run: npm run build
    
    - name: Build Docker image
      run: docker build -t myapp:${{ github.sha }} .
    
    - name: Push to registry
      if: github.ref == 'refs/heads/main'
      run: |
        echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin
        docker push myapp:${{ github.sha }}
```

**Exemple: Déploiement CD**

`.github/workflows/cd.yml`:

```yaml
name: CD Pipeline

on:
  push:
    branches: [ main ]
    tags:
      - 'v*'

jobs:
  deploy-staging:
    runs-on: ubuntu-latest
    environment: staging
    
    steps:
    - name: Checkout
      uses: actions/checkout@v3
    
    - name: Deploy to staging
      run: |
        ssh ${{ secrets.STAGING_SERVER }} "cd /app && git pull && docker-compose up -d"
  
  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment: production
    if: startsWith(github.ref, 'refs/tags/v')
    
    steps:
    - name: Deploy to production
      run: |
        ssh ${{ secrets.PROD_SERVER }} "cd /app && git pull && docker-compose up -d"
```

### 5.2.3 GitLab CI/CD

**`.gitlab-ci.yml`:**

```yaml
stages:
  - build
  - test
  - deploy

variables:
  DOCKER_IMAGE: registry.gitlab.com/myproject/app

# Build stage
build:
  stage: build
  image: maven:3.8-openjdk-17
  script:
    - mvn clean package
  artifacts:
    paths:
      - target/*.jar
    expire_in: 1 hour

# Test stage
unit-tests:
  stage: test
  image: maven:3.8-openjdk-17
  script:
    - mvn test
  coverage: '/Code coverage: \d+\.\d+%/'
  artifacts:
    reports:
      junit: target/surefire-reports/TEST-*.xml

integration-tests:
  stage: test
  image: maven:3.8-openjdk-17
  services:
    - postgres:14
    - redis:7
  variables:
    POSTGRES_DB: test_db
    POSTGRES_USER: test_user
    POSTGRES_PASSWORD: test_pass
  script:
    - mvn verify -P integration-tests

security-scan:
  stage: test
  image: owasp/dependency-check
  script:
    - dependency-check --scan . --format HTML --out reports
  artifacts:
    paths:
      - reports/
    expire_in: 1 week

# Deploy stages
deploy-staging:
  stage: deploy
  environment:
    name: staging
    url: https://staging.myapp.com
  only:
    - develop
  script:
    - kubectl set image deployment/myapp myapp=$DOCKER_IMAGE:$CI_COMMIT_SHA -n staging

deploy-production:
  stage: deploy
  environment:
    name: production
    url: https://myapp.com
  only:
    - tags
  when: manual  # Déploiement manuel requis
  script:
    - kubectl set image deployment/myapp myapp=$DOCKER_IMAGE:$CI_COMMIT_TAG -n production
```

### 5.2.4 Jenkins Pipeline

**`Jenkinsfile`:**

```groovy
pipeline {
    agent any
    
    tools {
        nodejs 'NodeJS 18'
    }
    
    environment {
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_IMAGE = 'mycompany/myapp'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/mycompany/myapp.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }
        
        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }
        
        stage('Unit Tests') {
            steps {
                sh 'npm test -- --coverage'
            }
            post {
                always {
                    junit 'test-results/*.xml'
                    publishHTML([
                        reportDir: 'coverage',
                        reportFiles: 'index.html',
                        reportName: 'Coverage Report'
                    ])
                }
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Docker Build') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }
        
        stage('Push to Registry') {
            when {
                branch 'main'
            }
            steps {
                script {
                    docker.withRegistry('https://docker.io', 'docker-credentials') {
                        docker.image("${DOCKER_IMAGE}:${BUILD_NUMBER}").push()
                        docker.image("${DOCKER_IMAGE}:${BUILD_NUMBER}").push('latest')
                    }
                }
            }
        }
        
        stage('Deploy to Staging') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                    ssh staging-server "docker pull ${DOCKER_IMAGE}:latest && \
                    docker-compose up -d"
                '''
            }
        }
        
        stage('Deploy to Production') {
            when {
                tag pattern: "v\\d+\\.\\d+\\.\\d+", comparator: "REGEXP"
            }
            steps {
                input message: 'Deploy to production?', ok: 'Deploy'
                sh '''
                    ssh prod-server "docker pull ${DOCKER_IMAGE}:${TAG_NAME} && \
                    docker-compose up -d"
                '''
            }
        }
    }
    
    post {
        success {
            slackSend color: 'good', 
                     message: "Build #${BUILD_NUMBER} succeeded!"
        }
        failure {
            slackSend color: 'danger', 
                     message: "Build #${BUILD_NUMBER} failed!"
        }
    }
}
```

### 5.2.5 Stratégies de déploiement

#### Blue-Green Deployment

```
┌─────────────────────────────────────────┐
│          Load Balancer                  │
└────────────┬────────────────────────────┘
             │
             │ 100% traffic
             ▼
     ┌──────────────┐         ┌──────────────┐
     │  Blue (v1.0) │         │ Green (v1.1) │
     │  ACTIVE      │         │  IDLE        │
     └──────────────┘         └──────────────┘

# Déploiement
1. Déployer v1.1 sur Green
2. Tester Green
3. Basculer traffic vers Green
4. Blue devient idle (rollback possible)

     ┌──────────────┐         ┌──────────────┐
     │  Blue (v1.0) │         │ Green (v1.1) │
     │  IDLE        │         │  ACTIVE      │
     └──────────────┘         └──────────────┘
             ▲                        ▲
             │                        │ 100% traffic
             │                        │
┌────────────┴────────────────────────┴────┐
│          Load Balancer                   │
└──────────────────────────────────────────┘
```

#### Canary Deployment

```
Étape 1: 5% vers nouvelle version
┌─────────────────────────┐
│     Load Balancer       │
└────┬───────────────┬────┘
     │ 95%           │ 5%
     ▼               ▼
┌─────────┐     ┌─────────┐
│ v1.0    │     │ v1.1    │
│ (stable)│     │ (canary)│
└─────────┘     └─────────┘

Étape 2: Si OK, augmenter progressivement
50% / 50%

Étape 3: 100% vers nouvelle version
┌─────────────────────────┐
│     Load Balancer       │
└────────────┬────────────┘
             │ 100%
             ▼
        ┌─────────┐
        │ v1.1    │
        └─────────┘
```

#### Rolling Deployment

```
Instances: [v1.0] [v1.0] [v1.0] [v1.0]

Étape 1:   [v1.1] [v1.0] [v1.0] [v1.0]
Étape 2:   [v1.1] [v1.1] [v1.0] [v1.0]
Étape 3:   [v1.1] [v1.1] [v1.1] [v1.0]
Étape 4:   [v1.1] [v1.1] [v1.1] [v1.1]
```

---

## 5.3 Collaboration entre équipes (frontend, backend, DevOps)

### 5.3.1 Contrat d'API (API Contract)

**OpenAPI / Swagger:**

```yaml
openapi: 3.0.0
info:
  title: E-commerce API
  version: 1.0.0
  description: API for e-commerce platform

servers:
  - url: https://api.myshop.com/v1

paths:
  /products:
    get:
      summary: List all products
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
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/Product'
                  pagination:
                    $ref: '#/components/schemas/Pagination'

  /products/{id}:
    get:
      summary: Get product by ID
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: Product found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Product'
        '404':
          description: Product not found

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
        description:
          type: string
        category:
          type: string
          example: "Electronics"
        stock:
          type: integer
          example: 50
    
    Pagination:
      type: object
      properties:
        currentPage:
          type: integer
        totalPages:
          type: integer
        totalItems:
          type: integer
```

**Utilisation:**
- Frontend et Backend développent en parallèle
- Documentation auto-générée
- Tests automatiques basés sur le contrat
- Mock servers pour développement isolé

### 5.3.2 API Mocking pour développement parallèle

**Prism (outil de mock basé sur OpenAPI):**

```bash
# Installer Prism
npm install -g @stoplight/prism-cli

# Démarrer le mock server
prism mock api-spec.yaml

# Le frontend peut maintenant appeler:
# http://localhost:4010/products
```

**Mock Service Worker (MSW) pour tests frontend:**

```javascript
// mocks/handlers.js
import { rest } from 'msw';

export const handlers = [
  rest.get('/api/products', (req, res, ctx) => {
    return res(
      ctx.status(200),
      ctx.json({
        data: [
          { id: 1, name: 'Product 1', price: 29.99 },
          { id: 2, name: 'Product 2', price: 39.99 }
        ],
        pagination: {
          currentPage: 1,
          totalPages: 5,
          totalItems: 100
        }
      })
    );
  }),
  
  rest.post('/api/orders', (req, res, ctx) => {
    return res(
      ctx.status(201),
      ctx.json({
        id: 123,
        status: 'PENDING',
        createdAt: new Date().toISOString()
      })
    );
  })
];

// Utilisation dans les tests
import { setupServer } from 'msw/node';
import { handlers } from './mocks/handlers';

const server = setupServer(...handlers);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

### 5.3.3 Communication inter-équipes

**Documentation partagée (Confluence, Notion):**

```markdown
# Feature: User Authentication

## Équipe Backend
- [ ] API endpoint POST /auth/login
- [ ] API endpoint POST /auth/logout
- [ ] JWT token generation
- [ ] Refresh token mechanism
- Deadline: 15/11/2024
- Contact: john@backend.team

## Équipe Frontend
- [ ] Login form component
- [ ] Token storage (localStorage)
- [ ] Axios interceptor for auth header
- [ ] Auto-refresh on token expiration
- Deadline: 18/11/2024
- Contact: sarah@frontend.team

## Équipe DevOps
- [ ] Redis pour session storage
- [ ] Rate limiting configuration
- [ ] SSL certificates
- Deadline: 12/11/2024
- Contact: mike@devops.team

## Dependencies
- Frontend blocked by Backend (API endpoints)
- DevOps to be ready before Backend deployment

## Risks
- Token expiration UX needs design input
- Rate limiting might affect automated tests
```

---

## 5.4 Bonnes pratiques de revue de code et documentation

### 5.4.1 Revue de code (Code Review)

**Checklist du reviewer:**

```markdown
## Fonctionnalité
- [ ] Le code fait ce qu'il est censé faire
- [ ] Les tests couvrent les cas limites
- [ ] Pas de régression introduite

## Qualité du code
- [ ] Code lisible et bien structuré
- [ ] Nommage clair (variables, fonctions, classes)
- [ ] Pas de duplication (DRY respecté)
- [ ] Complexité raisonnable (pas de fonctions > 50 lignes)
- [ ] Respect des conventions du projet

## Architecture
- [ ] Cohérent avec l'architecture existante
- [ ] Principes SOLID respectés
- [ ] Pas de couplage fort introduit
- [ ] Abstractions appropriées

## Performance
- [ ] Pas de boucles N+1
- [ ] Requêtes DB optimisées
- [ ] Pas de memory leaks
- [ ] Caching approprié

## Sécurité
- [ ] Pas de failles de sécurité (injection SQL, XSS, etc.)
- [ ] Validation des entrées utilisateur
- [ ] Authentification/autorisation correcte
- [ ] Données sensibles protégées

## Tests
- [ ] Tests unitaires présents et pertinents
- [ ] Tests d'intégration si nécessaire
- [ ] Coverage acceptable (> 80%)
- [ ] Tests lisibles et maintenables

## Documentation
- [ ] Code commenté quand nécessaire (pourquoi, pas quoi)
- [ ] README mis à jour si besoin
- [ ] API documentée (Swagger/JSDoc)
- [ ] Changelog mis à jour
```

**Exemples de commentaires constructifs:**

```
❌ Mauvais commentaire:
"Ce code est nul."

✅ Bon commentaire:
"Cette fonction fait trop de choses. Pourrait-on extraire la logique 
de validation dans une fonction séparée pour améliorer la lisibilité 
et la testabilité ?"

---

❌ Mauvais:
"Ça ne marchera jamais."

✅ Bon:
"Je pense qu'il y a un edge case non géré: que se passe-t-il si 
l'utilisateur envoie une liste vide ? Pourrait-on ajouter un test 
pour ce cas ?"

---

✅ Positif:
"Excellente abstraction ! L'utilisation du pattern Strategy rend 
ce code très extensible. 👍"

---

✅ Suggestion:
"Pour améliorer les performances, on pourrait envisager de cacher 
ce résultat. Qu'en penses-tu ?"
```

### 5.4.2 Pull Request (PR) / Merge Request (MR)

**Template de PR:**

```markdown
## Description
Brève description des changements.

## Type de changement
- [ ] Bug fix (non-breaking change qui corrige un problème)
- [ ] New feature (non-breaking change qui ajoute une fonctionnalité)
- [ ] Breaking change (changement qui casse la compatibilité)
- [ ] Refactoring (amélioration du code sans changement fonctionnel)
- [ ] Documentation

## Motivation et contexte
Pourquoi ce changement est-il nécessaire ? Quel problème résout-il ?

Fixes #123 (référence à l'issue)

## Comment a-t-il été testé ?
- [ ] Tests unitaires
- [ ] Tests d'intégration
- [ ] Tests manuels
- [ ] Tests E2E

Détails des tests effectués:
- Scénario 1: ...
- Scénario 2: ...

## Screenshots (si applicable)
Avant:
[screenshot]

Après:
[screenshot]

## Checklist
- [ ] Mon code suit le style du projet
- [ ] J'ai effectué une auto-revue
- [ ] J'ai commenté les parties complexes
- [ ] J'ai mis à jour la documentation
- [ ] Mes changements ne génèrent pas de nouveaux warnings
- [ ] J'ai ajouté des tests
- [ ] Tous les tests passent
- [ ] Les dépendances sont à jour

## Reviewers suggérés
@john-backend @sarah-frontend

## Notes additionnelles
Points d'attention particuliers pour les reviewers:
- La logique de cache dans `src/services/cacheService.js`
- Le nouveau endpoint API `/api/v2/users`
```

### 5.4.3 Documentation du code

**JSDoc (JavaScript/TypeScript):**

```javascript
/**
 * Calcule le prix total d'une commande avec réductions et taxes.
 * 
 * @param {Order} order - L'objet commande contenant les articles
 * @param {Customer} customer - Le client pour les réductions personnalisées
 * @param {Object} options - Options de calcul
 * @param {boolean} options.includeTax - Inclure les taxes (default: true)
 * @param {string} options.currency - Code devise ISO (default: 'CHF')
 * @returns {Promise<BigDecimal>} Le prix total calculé
 * @throws {InvalidOrderException} Si la commande est invalide
 * @throws {CustomerNotFoundException} Si le client n'existe pas
 * 
 * @example
 * const total = await calculateTotal(order, customer, { includeTax: true });
 * console.log(`Total: ${total} CHF`);
 * 
 * @since 1.2.0
 * @see {@link applyDiscount} pour les détails sur les réductions
 */
async function calculateTotal(order, customer, options = {}) {
    const { includeTax = true, currency = 'CHF' } = options;
    
    // Validation
    if (!order || !order.items || order.items.length === 0) {
        throw new InvalidOrderException('Order must contain at least one item');
    }
    
    // Calcul
    let total = order.items.reduce((sum, item) => 
        sum.add(item.price.multiply(item.quantity)), BigDecimal.ZERO
    );
    
    // Réductions
    const discount = await applyDiscount(total, customer);
    total = total.subtract(discount);
    
    // Taxes
    if (includeTax) {
        const tax = calculateTax(total, currency);
        total = total.add(tax);
    }
    
    return total;
}
```

**Javadoc (Java):**

```java
/**
 * Service de gestion des commandes.
 * 
 * <p>Cette classe fournit les opérations CRUD pour les commandes
 * et orchestre les processus métier associés (paiement, livraison, etc.).
 * 
 * <p>Exemple d'utilisation:
 * <pre>{@code
 * OrderService orderService = new OrderService(orderRepository, paymentService);
 * Order order = orderService.createOrder(request);
 * }</pre>
 * 
 * @author John Doe
 * @version 1.0
 * @since 2024-01-15
 */
@Service
public class OrderService {
    
    private final OrderRepository orderRepository;
    private final PaymentService paymentService;
    
    /**
     * Constructeur avec injection de dépendances.
     * 
     * @param orderRepository le repository pour persister les commandes
     * @param paymentService le service de paiement
     * @throws IllegalArgumentException si un paramètre est null
     */
    public OrderService(OrderRepository orderRepository, PaymentService paymentService) {
        this.orderRepository = Objects.requireNonNull(orderRepository, "orderRepository cannot be null");
        this.paymentService = Objects.requireNonNull(paymentService, "paymentService cannot be null");
    }
    
    /**
     * Crée une nouvelle commande et initie le processus de paiement.
     * 
     * @param request les détails de la commande à créer
     * @return l'objet Order créé avec son ID
     * @throws ValidationException si les données de la requête sont invalides
     * @throws PaymentException si le paiement échoue
     * @throws InsufficientStockException si le stock est insuffisant
     */
    @Transactional
    public Order createOrder(CreateOrderRequest request) {
        // Implémentation
    }
}
```

### 5.4.4 README.md du projet

**Template complet:**

```markdown
# Nom du Projet

[![Build Status](https://img.shields.io/github/workflow/status/user/repo/CI)](https://github.com/user/repo/actions)
[![Coverage](https://img.shields.io/codecov/c/github/user/repo)](https://codecov.io/gh/user/repo)
[![License](https://img.shields.io/github/license/user/repo)](LICENSE)

Brève description du projet en 1-2 phrases.

## 📋 Table des matières

- [Fonctionnalités](#fonctionnalités)
- [Prérequis](#prérequis)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [API Documentation](#api-documentation)
- [Tests](#tests)
- [Déploiement](#déploiement)
- [Technologies](#technologies)
- [Contributing](#contributing)
- [License](#license)

## ✨ Fonctionnalités

- 🔐 Authentification JWT
- 📦 Gestion de produits
- 🛒 Panier d'achat
- 💳 Intégration paiement (Stripe)
- 📧 Notifications email
- 📊 Dashboard administrateur

## 🔧 Prérequis

- Node.js >= 18.0.0
- PostgreSQL >= 14
- Redis >= 7 (pour le cache)
- Docker (optionnel)

## 📥 Installation

\`\`\`bash
# Cloner le repository
git clone https://github.com/user/repo.git
cd repo

# Installer les dépendances
npm install

# Copier le fichier de configuration
cp .env.example .env

# Lancer les migrations
npm run migrate

# Seeding (données de test)
npm run seed
\`\`\`

## 🚀 Usage

\`\`\`bash
# Mode développement
npm run dev

# Mode production
npm start

# Avec Docker
docker-compose up
\`\`\`

L'application sera accessible sur http://localhost:3000

## ⚙️ Configuration

Variables d'environnement (.env):

\`\`\`env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/mydb

# Redis
REDIS_URL=redis://localhost:6379

# JWT
JWT_SECRET=your-secret-key
JWT_EXPIRATION=24h

# Stripe
STRIPE_SECRET_KEY=sk_test_xxxxx
STRIPE_WEBHOOK_SECRET=whsec_xxxxx

# Email
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASSWORD=your-password
\`\`\`

## 📚 API Documentation

La documentation complète de l'API est disponible à:
- Swagger UI: http://localhost:3000/api-docs
- OpenAPI Spec: http://localhost:3000/api-docs.json

Exemples:

\`\`\`bash
# Créer un utilisateur
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "password123"}'

# Obtenir un token
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "password123"}'
\`\`\`

## 🧪 Tests

\`\`\`bash
# Tous les tests
npm test

# Tests unitaires
npm run test:unit

# Tests d'intégration
npm run test:integration

# Tests E2E
npm run test:e2e

# Coverage
npm run test:coverage
\`\`\`

## 🚢 Déploiement

### Heroku

\`\`\`bash
heroku create myapp
git push heroku main
heroku run npm run migrate
\`\`\`

### Docker

\`\`\`bash
docker build -t myapp .
docker run -p 3000:3000 --env-file .env myapp
\`\`\`

### Kubernetes

\`\`\`bash
kubectl apply -f k8s/
\`\`\`

## 🛠️ Technologies

- **Backend:** Node.js, Express, TypeScript
- **Database:** PostgreSQL, Prisma ORM
- **Cache:** Redis
- **Authentication:** JWT, Passport.js
- **Testing:** Jest, Supertest
- **CI/CD:** GitHub Actions
- **Containerization:** Docker

## 🤝 Contributing

Les contributions sont bienvenues ! Voir [CONTRIBUTING.md](CONTRIBUTING.md).

1. Fork le projet
2. Créer une branche (`git checkout -b feature/AmazingFeature`)
3. Commit (`git commit -m 'Add some AmazingFeature'`)
4. Push (`git push origin feature/AmazingFeature`)
5. Ouvrir une Pull Request

## 📝 License

Ce projet est sous licence MIT - voir [LICENSE](LICENSE) pour plus de détails.

## 👥 Auteurs

- **John Doe** - *Initial work* - [johndoe](https://github.com/johndoe)

## 🙏 Remerciements

- Hat tip à quiconque dont le code a été utilisé
- Inspiration
- etc
\`\`\`

---

## 📝 Résumé du chapitre

✅ **Git:** Outil essentiel de gestion de versions avec branches, merge, rebase

✅ **Workflows:** Git Flow (complet), GitHub Flow (simple), Trunk-Based (CI/CD optimal)

✅ **CI/CD:** Automatisation build, test, déploiement pour feedback rapide

✅ **Collaboration:** Contrats d'API, mocking, documentation partagée

✅ **Code Review:** Amélioration qualité, partage de connaissance, mentorat

✅ **Documentation:** README, JSDoc/Javadoc, comments pertinents

---

## 💡 Exercices pratiques

### Exercice 1: Git workflow
Créez un repository, implémentez une feature avec branches, PR et merge.

### Exercice 2: CI Pipeline
Configurez un pipeline GitHub Actions pour un projet Node.js avec tests et déploiement.

### Exercice 3: Code Review
Reviewez une PR fictive et donnez des commentaires constructifs.

---

## 📚 Pour aller plus loin

- 📖 **"Pro Git"** - Scott Chacon (gratuit en ligne)
- 📖 **"Continuous Delivery"** - Jez Humble
- 🔗 **git-scm.com**: Documentation officielle Git
- 🔗 **GitHub Skills**: Tutoriels interactifs

---

[⬅️ Chapitre 4](./chapitre-4-bonnes-pratiques-conception.md) | [➡️ Chapitre 6](./chapitre-6-developpement-qualite.md)

