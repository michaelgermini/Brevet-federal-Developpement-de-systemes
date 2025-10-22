# 📖 Index complet du manuel

> **Brevet fédéral Développement de systèmes — Architecture, conception et maintenance logicielle**

---

## 🔍 Table des matières détaillée

### [Introduction générale](./README.md)
- [Objectif du livre](./README.md#objectif-du-livre)
- [Public visé](./README.md#public-visé)
- [Structure du Brevet fédéral](./README.md#structure-du-brevet-fédéral-développement-de-systèmes)
- [Approche pédagogique](./README.md#approche-pédagogique-du-manuel)
- [Méthodologie pour la réussite](./README.md#méthodologie-pour-la-réussite-àlexamen)
- [Conseils de l'auteur](./README.md#conseils-de-lauteur)
- [Ressources complémentaires](./README.md#ressources-complémentaires)

---

## 📚 Partie I — Fondements du développement de systèmes

### [Chapitre 1: Cycle de vie logiciel](./partie-1-fondements/chapitre-1-cycle-vie-logiciel.md)
- [Définitions et terminologies clés](#11-définitions-et-terminologies-clés)
  - Qu'est-ce qu'un cycle de vie logiciel?
  - Phases universelles d'un projet logiciel
  - Terminologies essentielles
  - Métriques et indicateurs
- [Cycle en V, modèle en spirale, et méthodes itératives](#12-cycle-en-v-modèle-en-spirale-et-méthodes-itératives)
  - Modèle en cascade (Waterfall)
  - Cycle en V
  - Modèle en spirale
  - Méthodes itératives et incrémentales
- [Gestion des exigences et cahier des charges](#13-gestion-des-exigences-et-cahier-des-charges)
  - Types d'exigences (fonctionnelles/non fonctionnelles)
  - Collecte des exigences
  - Cahier des charges (SRS)
- [Lien entre analyse métier et conception technique](#14-lien-entre-analyse-métier-et-conception-technique)
  - Domain-Driven Design (DDD)
  - Event Storming
  - Traduction métier → technique

### [Chapitre 2: Analyse des besoins et spécifications](./partie-1-fondements/chapitre-2-analyse-besoins-specifications.md)
- [Collecte et formalisation des besoins utilisateurs](#21-collecte-et-formalisation-des-besoins-utilisateurs)
  - User stories et critères d'acceptation
  - Cas d'utilisation
- [Diagrammes UML](#22-diagrammes-uml-cas-dutilisation-séquences-classes)
  - Diagramme de cas d'utilisation
  - Diagramme de séquence
  - Diagramme de classes
- [Méthodes d'analyse fonctionnelle et technique](#23-méthodes-danalyse-fonctionnelle-et-technique)
  - MERISE (Europe)
  - Analyse CRUD
  - Analyse par Persona
- [Documentation des exigences (SRS) et validation](#24-documentation-des-exigences-srs-et-validation)
  - Techniques de validation
  - Revues par les pairs
  - Prototypage
  - Tests d'acceptation utilisateur

### [Chapitre 3: Architecture logicielle](./partie-1-fondements/chapitre-3-architecture-logicielle.md)
- [Principes de conception d'architectures logicielles](#31-principes-de-conception-darchitectures-logicielles)
  - Qualités architecturales (ISO 25010)
  - Principes fondamentaux
- [Modèles d'architecture](#32-modèles-darchitecture-monolithique-microservices-soa)
  - Architecture monolithique
  - SOA (Service-Oriented Architecture)
  - Microservices
  - Serverless
- [Patterns architecturaux](#33-patterns-architecturaux-mvc-cqrs-event-driven-layered)
  - Layered Architecture
  - MVC (Model-View-Controller)
  - CQRS (Command Query Responsibility Segregation)
  - Event-Driven Architecture
  - Hexagonal Architecture
- [Outils d'architecture](#34-outils-darchitecture-diagrammes-modélisation-et-documentation)
  - Architecture Decision Records (ADR)
  - C4 Model
  - Outils recommandés

---

## 📚 Partie II — Conception et développement

### [Chapitre 4: Bonnes pratiques de conception](./partie-2-conception-developpement/chapitre-4-bonnes-pratiques-conception.md)
- [Paradigmes de programmation](#41-paradigmes-de-programmation-procédural-objet-fonctionnel)
  - Programmation procédurale
  - Programmation orientée objet (POO)
  - Programmation fonctionnelle
- [Principes SOLID, DRY, KISS, YAGNI](#42-principes-solid-dry-kiss-yagni)
  - SOLID (Single Responsibility, Open/Closed, Liskov, Interface Segregation, Dependency Inversion)
  - DRY (Don't Repeat Yourself)
  - KISS (Keep It Simple, Stupid)
  - YAGNI (You Aren't Gonna Need It)
- [Patterns de conception](#43-patterns-de-conception-factory-singleton-observer-adapter)
  - Patterns créationnels (Factory, Singleton, Builder)
  - Patterns structurels (Adapter, Decorator)
  - Patterns comportementaux (Observer, Strategy)
- [Gestion de la dette technique et refactoring](#44-gestion-de-la-dette-technique-et-refactoring)
  - Code smells et indicateurs
  - Techniques de refactoring
  - Stratégies de gestion

### [Chapitre 5: Développement collaboratif](./partie-2-conception-developpement/chapitre-5-developpement-collaboratif.md)
- [Gestion de versions (Git, GitHub, GitLab, workflows)](#51-gestion-de-versions-git-github-gitlab-workflows)
  - Fondamentaux Git
  - Conventional Commits
  - Git Workflows (Git Flow, GitHub Flow, Trunk-Based)
  - Résolution de conflits
- [Intégration continue (CI/CD) et automatisation des tests](#52-intégration-continue-cicd-et-automatisation-des-tests)
  - Pipeline CI/CD
  - GitHub Actions, GitLab CI, Jenkins
  - Stratégies de déploiement (Blue-Green, Canary)
- [Collaboration entre équipes (frontend, backend, DevOps)](#53-collaboration-entre-équipes-frontend-backend-devops)
  - Contrat d'API (OpenAPI)
  - API Mocking pour développement parallèle
  - Communication inter-équipes
- [Bonnes pratiques de revue de code et documentation](#54-bonnes-pratiques-de-revue-de-code-et-documentation)
  - Checklist de revue
  - Pull Request / Merge Request
  - Documentation du code (JSDoc, Javadoc)

### [Chapitre 6: Développement orienté qualité](./partie-2-conception-developpement/chapitre-6-developpement-qualite.md)
- [Tests unitaires, d'intégration et fonctionnels](#61-tests-unitaires-dintégration-et-fonctionnels)
  - Pyramide des tests
  - Tests unitaires (JUnit, Jest, PyTest)
  - Tests d'intégration (Spring Boot, Testcontainers)
  - Tests E2E (Cypress, Selenium)
- [Stratégies de test : TDD, BDD](#62-stratégies-de-test-tdd-bdd)
  - TDD (Test-Driven Development) - Red-Green-Refactor
  - BDD (Behavior-Driven Development) - Gherkin, Cucumber
- [Outils de test](#63-outils-de-test-junit-pytest-jest-postman)
  - Tests d'API (Postman, Newman)
  - Tests de performance (JMeter, k6)
  - Tests de sécurité (OWASP ZAP)
- [Mesure de la couverture et validation logicielle](#64-mesure-de-la-couverture-et-validation-logicielle)
  - Code coverage (JaCoCo, Istanbul)
  - Analyse statique (SonarQube, ESLint)
  - Mutation testing

---

## 📚 Partie III — Technologies et frameworks

### [Chapitre 7: Développement web et API](./partie-3-technologies-frameworks/chapitre-7-developpement-web-api.md)
- [Architecture client-serveur et REST](#71-architecture-client-serveur-et-rest)
  - Modèle client-serveur
  - REST (Representational State Transfer)
  - Méthodes HTTP et codes de statut
- [Frameworks backend](#72-frameworks-backend-spring-laravel-express-django)
  - Spring Boot (Java)
  - Express.js (Node.js)
  - Laravel (PHP)
  - Django REST Framework (Python)
- [API RESTful et GraphQL](#73-api-restful-et-graphql)
  - Conception d'API REST
  - GraphQL (schéma, requêtes, resolvers)
  - REST vs GraphQL
- [Sécurité et authentification](#74-sécurité-et-authentification-jwt-oauth2-openid-connect)
  - JWT (JSON Web Token)
  - OAuth 2.0
  - HTTPS et headers de sécurité
  - Rate limiting

### [Chapitre 8: Développement desktop et mobile](./partie-3-technologies-frameworks/chapitre-8-developpement-desktop-mobile.md)
- [Applications natives et multiplateformes](#81-applications-natives-et-multiplateformes)
  - Native vs Multiplateforme
  - Trade-offs et choix
- [Frameworks modernes](#82-frameworks-modernes-qt-electron-flutter-react-native)
  - Flutter (Dart)
  - React Native (JavaScript/TypeScript)
  - Electron (Web technologies)
  - Qt (C++)
- [Gestion des performances et ressources](#83-gestion-des-performances-et-ressources)
  - Optimisation React Native / Flutter
  - Memory leaks et gestion mémoire
- [Connexion aux services cloud et aux API](#84-connexion-aux-services-cloud-et-aux-api)
  - Requêtes HTTP (Axios, http package)
  - Stockage local (AsyncStorage, shared_preferences)
  - Gestion des erreurs et offline

### [Chapitre 9: Bases de données et persistance](./partie-3-technologies-frameworks/chapitre-9-bases-de-donnees-persistance.md)
- [Modélisation relationnelle et normalisation](#91-modélisation-relationnelle-et-normalisation)
  - MCD/MLD (Modèle Conceptuel/Donné Logique)
  - Formes normales (1NF, 2NF, 3NF)
  - Dénormalisation stratégique
- [SQL avancé et ORM](#92-sql-avancé-et-orm-hibernate-sequelize-prisma)
  - Jointures, sous-requêtes, CTEs, Window Functions
  - Hibernate (Java), Sequelize (Node.js), Prisma (TypeScript)
- [Bases NoSQL](#93-bases-nosql-mongodb-redis-firebase-cassandra)
  - MongoDB (Document Store)
  - Redis (Cache/Key-Value)
  - Comparaison SQL vs NoSQL
- [Gestion des migrations et intégrité des données](#94-gestion-des-migrations-et-intégrité-des-données)
  - Migrations (Flyway, Liquibase)
  - Contraintes et triggers
  - Audit trails

---

## 📚 Partie IV — Sécurité, performance et maintenance

### [Chapitre 10: Sécurité logicielle](./partie-4-securite-performance-maintenance/chapitre-10-securite-logicielle.md)
- [Vulnérabilités courantes (OWASP Top 10)](#101-vulnérabilités-courantes-owasp-top-10)
  - Broken Access Control
  - Cryptographic Failures
  - Injection (SQL, NoSQL)
  - Insecure Design
  - Security Misconfiguration
  - Vulnerable Components
  - Identification/Authentication Failures
  - Software Integrity Failures
  - Security Logging Failures
  - SSRF (Server-Side Request Forgery)
- [Validation des entrées et gestion des sessions](#102-validation-des-entrées-et-gestion-des-sessions)
  - Validation côté serveur (Joi, Yup)
  - Sanitization (validator.js)
  - Sessions sécurisées (express-session + Redis)
  - CSRF Protection
- [Chiffrement et stockage sécurisé des données](#103-chiffrement-et-stockage-sécurisé-des-données)
  - Chiffrement symétrique (AES)
  - Chiffrement asymétrique (RSA)
  - Hachage sécurisé (bcrypt, Argon2)
- [Audit de sécurité et pentests](#104-audit-de-sécurité-et-pentests)
  - SAST (SonarQube, ESLint)
  - DAST (OWASP ZAP, Burp Suite)
  - Checklist d'audit

### [Chapitre 11: Optimisation et performance](./partie-4-securite-performance-maintenance/chapitre-11-optimisation-performance.md)
- [Profilage et diagnostic des performances](#111-profilage-et-diagnostic-des-performances)
  - Métriques clés
  - Outils de profilage (JProfiler, Clinic.js)
  - Diagnostic base de données (EXPLAIN ANALYZE)
- [Caching et optimisation de la mémoire](#112-caching-et-optimisation-de-la-mémoire)
  - Cache-Aside, Write-Through, Write-Behind
  - Cache HTTP et CDN
  - Object pooling
- [Scalabilité horizontale et verticale](#113-scalabilité-horizontale-et-verticale)
  - Scaling vertical (plus de ressources)
  - Scaling horizontal (load balancing)
  - Scaling base de données (replicas, sharding)
- [Observabilité et logs centralisés](#114-observabilité-et-logs-centralisés)
  - Les 3 piliers (Logs, Metrics, Traces)
  - ELK Stack, Prometheus + Grafana
  - Distributed tracing (OpenTelemetry)

### [Chapitre 12: Maintenance et support](./partie-4-securite-performance-maintenance/chapitre-12-maintenance-support.md)
- [Gestion des versions logicielles](#121-gestion-des-versions-logicielles)
  - Semantic Versioning (SemVer)
  - Changelog
  - Git Flow releases
- [Correctifs, patches et évolutions continues](#122-correctifs-patches-et-évolutions-continues)
  - Hotfix process
  - Blue-Green Deployment
  - Feature Flags
- [Documentation technique et fonctionnelle](#123-documentation-technique-et-fonctionnelle)
  - API documentation (OpenAPI/Swagger)
  - Architecture Decision Records (ADR)
  - Documentation utilisateur
- [Gestion des incidents et amélioration continue](#124-gestion-des-incidents-et-amélioration-continue)
  - Processus de gestion d'incident
  - Post-mortem
  - DORA Metrics
  - Amélioration continue

---

## 📚 Partie V — Gestion de projet et leadership technique

### [Chapitre 13: Méthodologies de travail](./partie-5-gestion-projet-leadership/chapitre-13-methodologies-travail.md)
- [Agile, Scrum, Kanban, DevOps](#131-agile-scrum-kanban-devops)
  - Manifeste Agile et principes
  - Scrum (roles, artefacts, événements)
  - Kanban (flux, WIP limits)
  - DevOps (culture, pratiques)
- [Planification et priorisation des tâches](#132-planification-et-priorisation-des-tâches)
  - MoSCoW
  - Value vs Effort Matrix
  - Roadmap produit
- [Estimation des charges et suivi d'avancement](#133-estimation-des-charges-et-suivi-davancement)
  - Story Points et Planning Poker
  - Vélocité
  - Burn-down Chart
- [Outils collaboratifs et communication d'équipe](#134-outils-collaboratifs-et-communication-déquipe)
  - Jira (gestion projet)
  - Confluence (documentation)
  - Slack/Teams (communication)
  - Daily standups, retrospectives

### [Chapitre 14: Leadership technique](./partie-5-gestion-projet-leadership/chapitre-14-leadership-technique.md)
- [Le rôle du lead developer / architecte](#141-le-rôle-du-lead-developer--architecte)
  - Responsabilités techniques et managériales
  - Compétences clés (techniques + soft skills)
- [Gestion de l'équipe et mentoring](#142-gestion-de-léquipe-et-mentoring)
  - One-on-One meetings
  - Mentoring (onboarding, pair programming)
  - Résolution de conflits
- [Coordination avec la direction IT et les métiers](#143-coordination-avec-la-direction-it-et-les-métiers)
  - Communication avec non-techniques
  - Gestion des attentes
  - Présentation à la direction
- [Culture de la qualité et innovation continue](#144-culture-de-la-qualité-et-innovation-continue)
  - Tech Talks, Hack Days, Innovation Sprints
  - Reconnaissance et célébration
  - Feedback constructif

---

## 📚 Partie VI — Projet du Brevet fédéral

### [Chapitre 15: Conception d'un projet de diplôme](./partie-6-projet-brevet/chapitre-15-conception-projet-diplome.md)
- [Choisir un sujet pertinent et réaliste](#151-choisir-un-sujet-pertinent-et-réaliste)
  - Critères de sélection
  - Exemples de sujets adaptés
  - Validation du sujet
- [Définir les objectifs, contraintes et livrables](#152-définir-les-objectifs-contraintes-et-livrables)
  - Cahier des charges
  - Work Breakdown Structure (WBS)
- [Rédiger la documentation technique](#153-rédiger-la-documentation-technique)
  - Structure du rapport technique
  - Conseils de rédaction
- [Préparer la soutenance orale](#154-préparer-la-soutenance-orale)
  - Structure de la présentation
  - Conseils pour la présentation
  - Questions du jury

### [Chapitre 16: Étude de cas complète](./partie-6-projet-brevet/chapitre-16-etude-de-cas-complete.md)
- [Phase d'analyse](#161-phase-danalyse)
  - Acteurs identifiés
  - Cas d'utilisation
  - Exigences fonctionnelles/non fonctionnelles
- [Architecture technique](#162-architecture-technique)
  - Vue d'ensemble
  - Stack technologique
  - Modèle de données
- [Implémentation technique](#163-implémentation-technique)
  - Backend (controllers, services, models)
  - Frontend (React components)
  - Tests (unitaires, intégration, E2E)
- [Résultats et métriques](#164-résultats-et-métriques)
  - Objectifs atteints
  - Défis surmontés
  - Évolutions futures
- [Évaluation critique](#165-évaluation-critique)
  - Points forts
  - Points d'amélioration

---

## 📚 Partie VII — Annexes et outils pratiques

### [Annexe A: Modèles UML et documentation technique](./partie-7-annexes/annexe-a-modeles-uml-documentation.md)
- Templates de diagrammes UML (Use Case, Class, Sequence)
- Templates ADR (Architecture Decision Records)
- Templates documentation architecture (C4 Model)
- Templates spécifications techniques

### [Annexe D: Liste d'outils recommandés](./partie-7-annexes/annexe-d-liste-outils-recommandes.md)
- Environnements de développement (IDE)
- Gestion de versions (Git, GitHub, GitLab)
- CI/CD (GitHub Actions, GitLab CI, Jenkins)
- Tests (JUnit, Jest, Cypress, JMeter)
- Qualité de code (SonarQube, ESLint)
- Sécurité (OWASP ZAP, Snyk)
- Monitoring (Prometheus, ELK, DataDog)
- Base de données (PostgreSQL, MongoDB, Redis)
- Conteneurisation (Docker, Kubernetes)
- Collaboration (Jira, Confluence, Slack)
- API (Postman, Swagger)
- Hébergement (Vercel, Render, DigitalOcean, AWS)

### [Annexe E: Ressources bibliographiques et normes](./partie-7-annexes/annexe-e-ressources-bibliographiques-normes.md)
- Livres essentiels (Clean Code, Clean Architecture, etc.)
- Sites web et blogs (MDN, Stack Overflow, Martin Fowler)
- Normes (ISO/IEC, IEEE, OWASP, RFC)
- Certifications (AWS, GCP, CISSP, Kubernetes)
- Communautés et événements (conférences, meetups, newsletters)
- Plateformes d'apprentissage (Coursera, Udemy, Pluralsight)

---

## 🔍 Recherche rapide par mot-clé

### Concepts clés
- [SOLID](./partie-2-conception-developpement/chapitre-4-bonnes-pratiques-conception.md#42-principes-solid-dry-kiss-yagni)
- [Design Patterns](./partie-2-conception-developpement/chapitre-4-bonnes-pratiques-conception.md#43-patterns-de-conception-factory-singleton-observer-adapter)
- [TDD/BDD](./partie-2-conception-developpement/chapitre-6-developpement-qualite.md#62-stratégies-de-test-tdd-bdd)
- [Microservices](./partie-1-fondements/chapitre-3-architecture-logicielle.md#32-modèles-darchitecture-monolithique-microservices-soa)
- [REST/GraphQL](./partie-3-technologies-frameworks/chapitre-7-developpement-web-api.md#73-api-restful-et-graphql)
- [OWASP Top 10](./partie-4-securite-performance-maintenance/chapitre-10-securite-logicielle.md#101-vulnérabilités-courantes-owasp-top-10)
- [Docker/Kubernetes](./partie-7-annexes/annexe-d-liste-outils-recommandes.md#d10-conteneurisation--orchestration)

### Technologies
- [Spring Boot](./partie-3-technologies-frameworks/chapitre-7-developpement-web-api.md#722-spring-boot-java)
- [React](./partie-3-technologies-frameworks/chapitre-8-developpement-desktop-mobile.md#822-react-native)
- [Flutter](./partie-3-technologies-frameworks/chapitre-8-developpement-desktop-mobile.md#821-flutter)
- [PostgreSQL](./partie-3-technologies-frameworks/chapitre-9-bases-de-donnees-persistance.md#91-modélisation-relationnelle-et-normalisation)
- [MongoDB](./partie-3-technologies-frameworks/chapitre-9-bases-de-donnees-persistance.md#931-mongodb-document-store)
- [Redis](./partie-3-technologies-frameworks/chapitre-9-bases-de-donnees-persistance.md#932-redis-cachekey-value-store)

### Méthodologies
- [Scrum](./partie-5-gestion-projet-leadership/chapitre-13-methodologies-travail.md#1311-scrum)
- [Kanban](./partie-5-gestion-projet-leadership/chapitre-13-methodologies-travail.md#1312-kanban)
- [Git Flow](./partie-2-conception-developpement/chapitre-5-developpement-collaboratif.md#511-git-workflows)
- [CI/CD](./partie-2-conception-developpement/chapitre-5-developpement-collaboratif.md#52-intégration-continue-cicd-et-automatisation-des-tests)

### Outils
- [GitHub Actions](./partie-2-conception-developpement/chapitre-5-developpement-collaboratif.md#521-github-actions)
- [Docker](./partie-7-annexes/annexe-d-liste-outils-recommandes.md#d10-conteneurisation--orchestration)
- [Kubernetes](./partie-7-annexes/annexe-d-liste-outils-recommandes.md#d10-conteneurisation--orchestration)
- [Postman](./partie-7-annexes/annexe-d-liste-outils-recommandes.md#d11-api-development)
- [Jira](./partie-5-gestion-projet-leadership/chapitre-13-methodologies-travail.md#134-outils-collaboratifs-et-communication-déquipe)

---

## 📊 Statistiques du manuel

- **7 parties** principales
- **16 chapitres** détaillés
- **5 annexes** pratiques
- **~300 pages** de contenu
- **Multi-langages** : Java, JavaScript, Python, TypeScript, SQL
- **Technologies couvertes** : 50+ frameworks et outils
- **Exemples concrets** : Code fonctionnel, diagrammes UML
- **Templates** : 20+ modèles réutilisables
- **Références** : 100+ ressources externes

---

## 🎯 Feuille de route d'apprentissage

### Niveau Débutant (0-6 mois)
1. [Chapitre 1](./partie-1-fondements/chapitre-1-cycle-vie-logiciel.md) - Cycle de vie logiciel
2. [Chapitre 4](./partie-2-conception-developpement/chapitre-4-bonnes-pratiques-conception.md) - Bonnes pratiques
3. [Chapitre 5](./partie-2-conception-developpement/chapitre-5-developpement-collaboratif.md) - Git & CI/CD
4. [Chapitre 6](./partie-2-conception-developpement/chapitre-6-developpement-qualite.md) - Tests
5. [Chapitre 7](./partie-3-technologies-frameworks/chapitre-7-developpement-web-api.md) - Web & APIs

### Niveau Intermédiaire (6-12 mois)
1. [Chapitre 2](./partie-1-fondements/chapitre-2-analyse-besoins-specifications.md) - Analyse & UML
2. [Chapitre 3](./partie-1-fondements/chapitre-3-architecture-logicielle.md) - Architecture
3. [Chapitre 8](./partie-3-technologies-frameworks/chapitre-8-developpement-desktop-mobile.md) - Mobile/Desktop
4. [Chapitre 9](./partie-3-technologies-frameworks/chapitre-9-bases-de-donnees-persistance.md) - Base de données
5. [Chapitre 10](./partie-4-securite-performance-maintenance/chapitre-10-securite-logicielle.md) - Sécurité

### Niveau Avancé (12-18 mois)
1. [Chapitre 11](./partie-4-securite-performance-maintenance/chapitre-11-optimisation-performance.md) - Performance
2. [Chapitre 12](./partie-4-securite-performance-maintenance/chapitre-12-maintenance-support.md) - Maintenance
3. [Chapitre 13](./partie-5-gestion-projet-leadership/chapitre-13-methodologies-travail.md) - Méthodologies
4. [Chapitre 14](./partie-5-gestion-projet-leadership/chapitre-14-leadership-technique.md) - Leadership
5. [Chapitre 15-16](./partie-6-projet-brevet/) - Projet de diplôme

---

*Ce manuel constitue une ressource complète pour maîtriser le développement de systèmes et réussir le Brevet fédéral. Bonne lecture et excellente réussite dans votre parcours professionnel !*

---

[⬅️ Retour à l'accueil](./README.md) | [🔝 Haut de page](#-index-complet-du-manuel)

