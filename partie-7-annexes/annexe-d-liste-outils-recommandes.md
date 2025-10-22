# Annexe D : Liste d'outils recommandés

Guide complet des outils essentiels pour le développement professionnel, avec comparatifs et recommandations d'utilisation.

---

## D.1 Environnements de développement (IDE)

### Visual Studio Code
- **Type:** Éditeur léger et extensible
- **Langages:** Tous (via extensions)
- **✅ Points forts:** Gratuit, léger, énorme écosystème d'extensions, Git intégré
- **❌ Limitations:** Nécessite configuration pour projets complexes
- **💰 Prix:** Gratuit
- **🔗 Site:** code.visualstudio.com

### IntelliJ IDEA
- **Type:** IDE Java complet
- **Langages:** Java, Kotlin, Scala, Groovy
- **✅ Points forts:** Refactoring puissant, debugging avancé, Maven/Gradle intégré
- **❌ Limitations:** Consommation mémoire élevée, courbe d'apprentissage
- **💰 Prix:** Gratuit (Community), 149-499€/an (Ultimate)
- **🔗 Site:** jetbrains.com/idea

### PyCharm
- **Type:** IDE Python spécialisé
- **Langages:** Python
- **✅ Points forts:** Complétion intelligente, debugging, intégration scientifique (Jupyter)
- **❌ Limitations:** Lourd pour petits scripts
- **💰 Prix:** Gratuit (Community), 89-199€/an (Professional)
- **🔗 Site:** jetbrains.com/pycharm

---

## D.2 Gestion de versions

### Git + GitHub
- **✅ Recommandation:** Standard industrie
- **Features:** Repositories, Pull Requests, Actions (CI/CD), Issues, Projects
- **💰 Prix:** Gratuit (public + private), Pro 4$/mois
- **🔗 Site:** github.com

### GitLab
- **✅ Recommandation:** Alternative complète avec CI/CD intégré
- **Features:** Repos, CI/CD, Container Registry, Security scanning
- **💰 Prix:** Gratuit, Premium 19$/user/mois
- **🔗 Site:** gitlab.com

### Bitbucket
- **✅ Recommandation:** Bonne intégration avec Atlassian (Jira)
- **Features:** Repos, Pipelines, Jira integration
- **💰 Prix:** Gratuit (5 users), 3$/user/mois
- **🔗 Site:** bitbucket.org

---

## D.3 CI/CD

### GitHub Actions
- **✅ Usage:** CI/CD directement dans GitHub
- **Features:** Workflows YAML, Marketplace d'actions, Matrix builds
- **💰 Prix:** 2000 min/mois gratuit, puis 0.008$/min
- **🔗 Docs:** docs.github.com/actions

### GitLab CI/CD
- **✅ Usage:** Intégré à GitLab, très puissant
- **Features:** Pipelines, Auto DevOps, Container Registry
- **💰 Prix:** Inclus dans GitLab Free
- **🔗 Docs:** docs.gitlab.com/ee/ci

### Jenkins
- **✅ Usage:** Solution open-source auto-hébergée
- **Features:** Très flexible, énorme écosystème plugins
- **❌ Limitations:** Configuration complexe, maintenance nécessaire
- **💰 Prix:** Gratuit (open-source)
- **🔗 Site:** jenkins.io

---

## D.4 Tests

### Backend

**JUnit 5 (Java)**
- Standard pour tests Java
- 🔗 junit.org/junit5

**Jest (JavaScript/TypeScript)**
- Framework tout-en-un (unit + integration + coverage)
- 🔗 jestjs.io

**PyTest (Python)**
- Simple et puissant, plugins riches
- 🔗 pytest.org

**Postman / Newman**
- Tests API manuels et automatisés
- 🔗 postman.com

### Frontend

**Jest + React Testing Library**
- Standard React
- 🔗 testing-library.com/react

**Cypress**
- Tests E2E modernes, debugging excellent
- 🔗 cypress.io

**Playwright**
- Alternative Cypress, multi-browser natif
- 🔗 playwright.dev

### Performance

**JMeter**
- Tests de charge classiques
- 🔗 jmeter.apache.org

**k6**
- Tests de charge modernes (JavaScript)
- 🔗 k6.io

**Lighthouse**
- Performance web (Google)
- 🔗 developers.google.com/web/tools/lighthouse

---

## D.5 Qualité de code

### SonarQube / SonarCloud
- **Usage:** Analyse statique continue
- **Features:** Bugs, code smells, security vulnerabilities, coverage
- **💰 Prix:** SonarCloud gratuit (public), SonarQube self-hosted
- **🔗 Site:** sonarqube.org

### ESLint (JavaScript/TypeScript)
- **Usage:** Linter standard JS/TS
- **Features:** Règles configurables, auto-fix
- **💰 Prix:** Gratuit
- **🔗 Site:** eslint.org

### Prettier
- **Usage:** Code formatter opinionated
- **Features:** Format automatique, intégration IDE
- **💰 Prix:** Gratuit
- **🔗 Site:** prettier.io

### Checkstyle / SpotBugs (Java)
- **Usage:** Standards Java
- **💰 Prix:** Gratuit
- **🔗 Site:** checkstyle.org, spotbugs.github.io

---

## D.6 Sécurité

### OWASP ZAP
- **Usage:** Security scanner open-source
- **Features:** Automated + manual scanning
- **💰 Prix:** Gratuit
- **🔗 Site:** zaproxy.org

### Snyk
- **Usage:** Scan dépendances, containers, code
- **Features:** Alertes vulnérabilités, fix automatiques
- **💰 Prix:** Gratuit (open-source), Team 57$/dev/mois
- **🔗 Site:** snyk.io

### Dependabot (GitHub)
- **Usage:** Updates automatiques dépendances
- **Features:** Pull requests automatiques
- **💰 Prix:** Gratuit (intégré GitHub)
- **🔗 Docs:** docs.github.com/code-security/dependabot

---

## D.7 Monitoring & Observabilité

### Prometheus + Grafana
- **Usage:** Métriques time-series + dashboards
- **Features:** Alerting, data exploration
- **💰 Prix:** Gratuit (open-source)
- **🔗 Site:** prometheus.io, grafana.com

### ELK Stack (Elasticsearch, Logstash, Kibana)
- **Usage:** Logs centralisés, recherche, visualisation
- **💰 Prix:** Gratuit (self-hosted), Cloud payant
- **🔗 Site:** elastic.co

### DataDog
- **Usage:** APM, logs, metrics, tout-en-un
- **Features:** Dashboards, alerting, tracing
- **💰 Prix:** 15-31$/host/mois
- **🔗 Site:** datadoghq.com

### New Relic
- **Usage:** Similar DataDog
- **💰 Prix:** Gratuit (100GB/mois), Pro 0.25$/GB
- **🔗 Site:** newrelic.com

---

## D.8 Base de données

### PostgreSQL
- **Usage:** RDBMS open-source de référence
- **✅ Recommandation:** Défaut pour SQL
- **💰 Prix:** Gratuit
- **🔗 Site:** postgresql.org

### MongoDB
- **Usage:** Document database NoSQL
- **✅ Recommandation:** Données semi-structurées
- **💰 Prix:** Gratuit (Community), Atlas gratuit (512MB)
- **🔗 Site:** mongodb.com

### Redis
- **Usage:** Cache in-memory, pub/sub
- **✅ Recommandation:** Cache + sessions
- **💰 Prix:** Gratuit (open-source)
- **🔗 Site:** redis.io

### DBeaver
- **Usage:** Client SQL universel
- **✅ Recommandation:** Meilleur client gratuit
- **💰 Prix:** Gratuit (Community), 10$/mois (Enterprise)
- **🔗 Site:** dbeaver.io

---

## D.9 Conteneurisation & Orchestration

### Docker
- **Usage:** Conteneurisation applications
- **✅ Recommandation:** Standard industrie
- **💰 Prix:** Gratuit (Docker Desktop limité commercial)
- **🔗 Site:** docker.com

### Docker Compose
- **Usage:** Orchestration multi-containers local
- **✅ Recommandation:** Développement local
- **💰 Prix:** Gratuit
- **🔗 Docs:** docs.docker.com/compose

### Kubernetes
- **Usage:** Orchestration production
- **✅ Recommandation:** Production scale
- **💰 Prix:** Gratuit (k8s), cloud managed payant
- **🔗 Site:** kubernetes.io

---

## D.10 Collaboration & Gestion de projet

### Jira
- **Usage:** Gestion projet Agile
- **Features:** Sprints, Kanban, Roadmaps
- **💰 Prix:** Gratuit (10 users), 7.75$/user/mois
- **🔗 Site:** atlassian.com/software/jira

### Confluence
- **Usage:** Documentation collaborative
- **Features:** Pages, spaces, templates
- **💰 Prix:** Gratuit (10 users), 5.75$/user/mois
- **🔗 Site:** atlassian.com/software/confluence

### Notion
- **Usage:** Workspace tout-en-un
- **Features:** Notes, bases de données, wiki
- **💰 Prix:** Gratuit (personal), 8$/user/mois
- **🔗 Site:** notion.so

### Slack
- **Usage:** Communication équipe
- **Features:** Channels, threads, integrations
- **💰 Prix:** Gratuit (limité), Pro 7.25$/user/mois
- **🔗 Site:** slack.com

### Microsoft Teams
- **Usage:** Alternative Slack (Microsoft)
- **💰 Prix:** Gratuit (limité), inclus Microsoft 365
- **🔗 Site:** microsoft.com/teams

---

## D.11 API Development

### Postman
- **Usage:** Test et documentation API
- **Features:** Collections, environments, mock servers
- **💰 Prix:** Gratuit, Team 12$/user/mois
- **🔗 Site:** postman.com

### Insomnia
- **Usage:** Alternative Postman, plus simple
- **💰 Prix:** Gratuit
- **🔗 Site:** insomnia.rest

### Swagger / OpenAPI
- **Usage:** Documentation API standard
- **Features:** Spec, UI, Codegen
- **💰 Prix:** Gratuit (open-source)
- **🔗 Site:** swagger.io

---

## D.12 Hébergement & Cloud

### Hébergement gratuit/peu cher (projets personnels/démos)

**Vercel**
- Frontend (Next.js, React, Vue)
- Gratuit (hobby), Pro 20$/mois
- 🔗 vercel.com

**Netlify**
- Static sites + Serverless
- Gratuit (hobby), Pro 19$/mois
- 🔗 netlify.com

**Render**
- Web services, Databases, Static sites
- Gratuit (limité), 7$/mois
- 🔗 render.com

**Railway**
- Full-stack apps, databases
- 5$/mois + usage
- 🔗 railway.app

**DigitalOcean**
- VPS (Droplets), Kubernetes
- 5-6$/mois (basic droplet)
- 🔗 digitalocean.com

### Cloud majeurs (production entreprise)

**AWS (Amazon Web Services)**
- Le plus complet, complexe
- 🔗 aws.amazon.com

**Google Cloud Platform (GCP)**
- Bon pour ML/Data, Kubernetes
- 🔗 cloud.google.com

**Microsoft Azure**
- Intégration Microsoft, .NET
- 🔗 azure.microsoft.com

---

## D.13 Design & Prototyping

### Figma
- **Usage:** Design UI/UX collaboratif
- **Features:** Prototyping, design system
- **💰 Prix:** Gratuit (3 projets), Pro 12$/editor/mois
- **🔗 Site:** figma.com

### Draw.io / Diagrams.net
- **Usage:** Diagrammes (UML, architecture, flowcharts)
- **💰 Prix:** Gratuit
- **🔗 Site:** diagrams.net

### Excalidraw
- **Usage:** Diagrammes hand-drawn rapides
- **💰 Prix:** Gratuit
- **🔗 Site:** excalidraw.com

---

## D.14 Productivité

### Obsidian
- **Usage:** Notes markdown, knowledge base
- **💰 Prix:** Gratuit (personal)
- **🔗 Site:** obsidian.md

### TablePlus
- **Usage:** Client base de données moderne
- **💰 Prix:** Gratuit (limité), 59$ (lifetime)
- **🔗 Site:** tableplus.com

### Oh My Zsh
- **Usage:** Framework terminal (Zsh)
- **💰 Prix:** Gratuit
- **🔗 Site:** ohmyz.sh

---

## 📊 Matrice de sélection rapide

| Besoin | Outil recommandé | Alternative |
|--------|------------------|-------------|
| **IDE polyvalent** | VS Code | IntelliJ IDEA |
| **IDE Java** | IntelliJ IDEA | Eclipse |
| **IDE Python** | PyCharm | VS Code + extensions |
| **Git hosting** | GitHub | GitLab |
| **CI/CD** | GitHub Actions | GitLab CI |
| **Tests backend** | Jest / JUnit / PyTest | - |
| **Tests E2E** | Cypress | Playwright |
| **Tests API** | Postman | Insomnia |
| **Qualité code** | SonarQube | ESLint + Prettier |
| **Sécurité** | Snyk | OWASP ZAP |
| **Monitoring** | Prometheus + Grafana | DataDog |
| **Logs** | ELK Stack | Loki + Grafana |
| **Database SQL** | PostgreSQL | MySQL |
| **Database NoSQL** | MongoDB | DynamoDB |
| **Cache** | Redis | Memcached |
| **Containers** | Docker | Podman |
| **Orchestration** | Kubernetes | Docker Swarm |
| **Project mgmt** | Jira | Linear |
| **Documentation** | Confluence | Notion |
| **Communication** | Slack | Teams |
| **Hosting (petits projets)** | Render | Railway |
| **Cloud (entreprise)** | AWS | GCP |

---

[⬅️ Annexe C](./annexe-c-exemples-planification-projet.md) | [➡️ Annexe E](./annexe-e-ressources-bibliographiques-normes.md)

