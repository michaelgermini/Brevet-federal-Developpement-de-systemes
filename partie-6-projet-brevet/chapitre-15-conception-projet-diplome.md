# Chapitre 15 : Conception d'un projet de diplôme

> **Objectif:** Définir et planifier un projet de diplôme réaliste, pertinent et conforme aux exigences du Brevet fédéral.

---

## 📋 Table des matières

1. [Choisir un sujet pertinent et réaliste](#151-choisir-un-sujet-pertinent-et-réaliste)
2. [Définir les objectifs, contraintes et livrables](#152-définir-les-objectifs-contraintes-et-livrables)
3. [Rédiger la documentation technique](#153-rédiger-la-documentation-technique)
4. [Préparer la soutenance orale](#154-préparer-la-soutenance-orale)

---

## 15.1 Choisir un sujet pertinent et réaliste

### 15.1.1 Critères d'un bon sujet

**✅ Doit démontrer:**
- Compétences techniques variées (backend, frontend, DB, etc.)
- Capacité de conception (architecture, modélisation)
- Gestion de projet (planning, priorisation)
- Résolution de problèmes complexes
- Qualité professionnelle (tests, documentation, sécurité)

**✅ Doit être:**
- Réalisable en 3-6 mois (150-200h de travail)
- Ni trop simple, ni trop ambitieux
- Pertinent professionnellement
- Mesurable (objectifs quantifiables)

**❌ À éviter:**
- Projets trop théoriques
- Sujets sans valeur métier claire
- Scope trop large (impossible à terminer)
- Technologie unique (manque de diversité)
- Duplication de systèmes existants sans valeur ajoutée

### 15.1.2 Exemples de sujets adaptés

**1. Système de gestion événementielle**

```
Description:
Plateforme pour organiser et gérer des événements (conférences, meetups)

Fonctionnalités:
- Inscription participants
- Gestion billets et paiements
- Planification sessions
- Check-in QR code
- Statistiques organisateurs

Technologies:
- Backend: Node.js + Express + PostgreSQL
- Frontend: React
- Mobile: React Native (check-in app)
- Infra: Docker + CI/CD

Complexité: Moyenne-élevée
Durée estimée: 4-5 mois
```

**2. Application de suivi de fitness**

```
Description:
App mobile pour suivre exercices, nutrition et progrès

Fonctionnalités:
- Bibliothèque exercices avec vidéos
- Planification entraînements
- Tracking nutrition avec scan codes-barres
- Graphiques progression
- Intégration appareils connectés

Technologies:
- Backend: Django REST API + PostgreSQL
- Mobile: Flutter
- ML: Reconnaissance aliments (TensorFlow Lite)
- Infra: AWS + Docker

Complexité: Élevée
Durée estimée: 5-6 mois
```

**3. Plateforme de gestion de location immobilière**

```
Description:
Système pour propriétaires gérant plusieurs locations

Fonctionnalités:
- Gestion biens et locataires
- Contrats et documents
- Suivi paiements et rappels
- Calendrier maintenance
- Génération rapports financiers

Technologies:
- Backend: Spring Boot + PostgreSQL
- Frontend: Angular
- API: REST + GraphQL
- Reporting: Jasper Reports
- Infra: Kubernetes

Complexité: Moyenne
Durée estimée: 4 mois
```

### 15.1.3 Validation du sujet

**Questions à se poser:**

```markdown
## Faisabilité
- [ ] Ai-je les compétences techniques nécessaires?
- [ ] Le scope est-il réaliste pour le délai?
- [ ] Puis-je obtenir les ressources nécessaires (APIs, données)?

## Pertinence
- [ ] Le projet démontre-t-il des compétences de niveau Brevet fédéral?
- [ ] Y a-t-il une vraie problématique métier?
- [ ] Le projet a-t-il de la valeur professionnelle?

## Diversité technique
- [ ] Backend + Frontend + Base de données?
- [ ] Sécurité et authentification?
- [ ] Tests automatisés?
- [ ] CI/CD et déploiement?

## Documentation
- [ ] Puis-je documenter clairement l'architecture?
- [ ] Ai-je assez de matière pour la soutenance?
- [ ] Les choix techniques sont-ils justifiables?
```

---

## 15.2 Définir les objectifs, contraintes et livrables

### 15.2.1 Cahier des charges

**Structure type:**

```markdown
# Cahier des Charges - [Titre Projet]

## 1. Contexte et problématique

### 1.1 Contexte
[Description du domaine, des acteurs, de la situation actuelle]

### 1.2 Problématique
[Quel problème le projet résout-il?]

### 1.3 Objectifs généraux
1. Objectif principal
2. Objectifs secondaires
3. Objectifs d'apprentissage

## 2. Analyse des besoins

### 2.1 Acteurs
- **Utilisateur final**: [Description]
- **Administrateur**: [Description]
- **[Autre rôle]**: [Description]

### 2.2 Exigences fonctionnelles
RF-001: [Description]
RF-002: [Description]
...

### 2.3 Exigences non fonctionnelles
RNF-001: Performance (temps de réponse < 2s)
RNF-002: Sécurité (chiffrement données sensibles)
RNF-003: Disponibilité (99% uptime)
...

## 3. Architecture technique

### 3.1 Vue d'ensemble
[Diagramme d'architecture]

### 3.2 Technologies
- Backend: [Choix + justification]
- Frontend: [Choix + justification]
- Base de données: [Choix + justification]
- Infrastructure: [Choix + justification]

### 3.3 Justification des choix techniques
[Pour chaque technologie majeure, expliquer pourquoi]

## 4. Livrables

### 4.1 Livrables techniques
- [ ] Application fonctionnelle
- [ ] Code source (GitHub)
- [ ] Tests automatisés (coverage > 80%)
- [ ] Documentation API (Swagger)
- [ ] Scripts déploiement (Docker Compose)

### 4.2 Livrables documentaires
- [ ] Rapport technique complet (40-60 pages)
- [ ] Diagrammes UML (use cases, classes, séquences)
- [ ] Architecture Decision Records
- [ ] Manuel utilisateur
- [ ] Guide d'installation

### 4.3 Présentation
- [ ] Slides PowerPoint/PDF
- [ ] Démonstration live
- [ ] Vidéo backup (si démo live impossible)

## 5. Planning

### 5.1 Phases du projet
Phase 1: Analyse et conception (3 semaines)
Phase 2: Développement backend (6 semaines)
Phase 3: Développement frontend (5 semaines)
Phase 4: Tests et corrections (3 semaines)
Phase 5: Documentation (3 semaines)
Phase 6: Préparation soutenance (1 semaine)

Total: 21 semaines (~5 mois)

### 5.2 Jalons
- Fin Phase 1: Spécifications validées
- Fin Phase 2: API fonctionnelle
- Mi-Phase 3: MVP utilisable
- Fin Phase 4: Version stable
- Fin Phase 5: Documentation complète
- Fin Phase 6: Soutenance

## 6. Contraintes

### 6.1 Contraintes techniques
- Budget serveur: 0 CHF (hébergement gratuit)
- Pas d'accès à APIs payantes
- Développement solo

### 6.2 Contraintes temporelles
- Début: 01/11/2024
- Rendu: 31/03/2025
- Soutenance: Avril 2025

### 6.3 Risques identifiés
| Risque | Probabilité | Impact | Mitigation |
|--------|-------------|--------|------------|
| Complexité technique sous-estimée | Moyen | Élevé | Buffer 20% dans planning |
| Panne serveur avant démo | Faible | Élevé | Vidéo backup + local fallback |
| Maladie/imprévu | Moyen | Moyen | Avance prise, communication jury |
```

### 15.2.2 Work Breakdown Structure (WBS)

```
Projet: Plateforme Événementielle
├─ 1. Analyse & Conception
│  ├─ 1.1 Analyse besoins
│  ├─ 1.2 Modélisation UML
│  ├─ 1.3 Maquettes UI/UX
│  └─ 1.4 Architecture technique
├─ 2. Backend API
│  ├─ 2.1 Setup projet + DB
│  ├─ 2.2 Auth & User management
│  ├─ 2.3 Events module
│  ├─ 2.4 Tickets & Payments
│  └─ 2.5 Tests backend
├─ 3. Frontend Web
│  ├─ 3.1 Setup + routing
│  ├─ 3.2 Components UI
│  ├─ 3.3 Intégration API
│  └─ 3.4 Tests frontend
├─ 4. Mobile App
│  ├─ 4.1 Setup Flutter
│  ├─ 4.2 QR Code scanner
│  ├─ 4.3 Check-in flow
│  └─ 4.4 Tests mobile
├─ 5. DevOps & Deployment
│  ├─ 5.1 CI/CD pipeline
│  ├─ 5.2 Docker containers
│  └─ 5.3 Monitoring setup
├─ 6. Tests & Qualité
│  ├─ 6.1 Tests automatisés
│  ├─ 6.2 Tests E2E
│  └─ 6.3 Bug fixes
├─ 7. Documentation
│  ├─ 7.1 Rapport technique
│  ├─ 7.2 API documentation
│  └─ 7.3 Manuel utilisateur
└─ 8. Soutenance
   ├─ 8.1 Slides présentation
   ├─ 8.2 Démo préparée
   └─ 8.3 Répétitions
```

---

## 15.3 Rédiger la documentation technique

### 15.3.1 Structure du rapport technique

```markdown
# Rapport Technique - [Titre Projet]

## Page de garde
- Titre du projet
- Nom du candidat
- Session d'examen
- Date

## Table des matières

## 1. Introduction (3-5 pages)
1.1 Contexte du projet
1.2 Problématique
1.3 Objectifs
1.4 Structure du document

## 2. Analyse (8-12 pages)
2.1 Étude de l'existant
2.2 Analyse des besoins
    - Acteurs
    - Cas d'utilisation
    - Diagrammes UML
2.3 Spécifications fonctionnelles
2.4 Spécifications techniques

## 3. Conception (10-15 pages)
3.1 Architecture logicielle
    - Vue d'ensemble
    - Diagrammes C4
    - Patterns utilisés
3.2 Conception détaillée
    - Modèle de données
    - Diagrammes de classes
    - Diagrammes de séquence
3.3 Choix technologiques
    - Technologies retenues
    - Justifications
    - Alternatives considérées (ADRs)

## 4. Réalisation (12-18 pages)
4.1 Environnement de développement
4.2 Backend
    - Structure du code
    - API REST
    - Base de données
4.3 Frontend
    - Architecture composants
    - State management
    - UI/UX
4.4 Tests
    - Stratégie de test
    - Couverture
    - Résultats
4.5 DevOps
    - CI/CD
    - Déploiement
    - Monitoring

## 5. Résultats et Validation (5-8 pages)
5.1 Fonctionnalités réalisées
5.2 Tests utilisateurs
5.3 Métriques
    - Performance
    - Qualité code
    - Couverture tests
5.4 Écarts avec objectifs initiaux

## 6. Évaluation critique (4-6 pages)
6.1 Points forts du projet
6.2 Difficultés rencontrées et solutions
6.3 Limites et améliorations futures
6.4 Apprentissages personnels

## 7. Conclusion (2-3 pages)
7.1 Bilan du projet
7.2 Perspectives
7.3 Compétences développées

## Annexes
A. Code source (extraits significatifs)
B. Résultats des tests
C. Manuel utilisateur
D. Guide d'installation
E. Glossaire
F. Bibliographie
```

### 15.3.2 Conseils de rédaction

**Style:**
- ✅ Professionnel mais accessible
- ✅ Phrases courtes et claires
- ✅ Éviter le jargon excessif (ou l'expliquer)
- ✅ Utiliser des listes à puces pour la clarté
- ✅ Illustrer avec des diagrammes et captures d'écran

**Diagrammes:**
- ✅ UML propres et lisibles (PlantUML, Draw.io)
- ✅ Légendes explicites
- ✅ Taille adaptée (pas trop petits)
- ✅ Numérotés et référencés dans le texte

**Code:**
- ✅ Extraits pertinents uniquement (pas tout le code)
- ✅ Bien formatés avec syntax highlighting
- ✅ Commentés si nécessaire
- ✅ Référence au repository GitHub pour le code complet

---

## 15.4 Préparer la soutenance orale

### 15.4.1 Structure de la présentation (20-30 min)

```
1. Introduction (2 min)
   - Qui je suis
   - Contexte du projet
   - Problématique

2. Démonstration (8-10 min)
   - Scénario utilisateur complet
   - Fonctionnalités clés
   - Points techniques remarquables

3. Architecture & Conception (5-7 min)
   - Diagramme architecture
   - Choix technologiques (justifiés)
   - Patterns utilisés

4. Réalisation technique (3-5 min)
   - Extraits de code significatifs
   - Défis techniques surmontés
   - Tests et qualité

5. Bilan et perspectives (2-3 min)
   - Objectifs atteints
   - Difficultés et apprentissages
   - Évolutions futures

6. Questions/Réponses (10-15 min)
```

### 15.4.2 Conseils pour la présentation

**Préparation:**
- ✅ Répéter plusieurs fois (chronométrer)
- ✅ Préparer démo alternative (vidéo) si problème technique
- ✅ Anticiper questions probables du jury
- ✅ Tester équipement (projecteur, micro, connexion)

**Pendant la soutenance:**
- ✅ Parler clairement, pas trop vite
- ✅ Regarder le jury, pas seulement les slides
- ✅ Montrer l'enthousiasme pour le projet
- ✅ Rester humble et honnête sur les limites

**Questions du jury:**
- ✅ Écouter la question jusqu'au bout
- ✅ Reformuler si besoin ("Si je comprends bien...")
- ✅ Répondre honnêtement ("Je ne sais pas" acceptable)
- ✅ Relier à des éléments du projet quand possible

**Questions typiques:**
```
- Pourquoi avoir choisi [technologie X]?
- Comment avez-vous géré [problème Y]?
- Qu'auriez-vous fait différemment?
- Comment scaleriez-vous cette solution?
- Quelles sont les failles de sécurité potentielles?
- Comment testez-vous [fonctionnalité Z]?
- Quel a été le plus grand défi technique?
```

---

## 📝 Résumé

✅ **Sujet:** Pertinent, réaliste, démontrant compétences variées

✅ **Cahier des charges:** Objectifs clairs, contraintes, livrables définis

✅ **Planning:** WBS détaillé, jalons, buffer pour imprévus

✅ **Documentation:** Rapport technique ~50 pages, diagrammes, code

✅ **Soutenance:** Présentation structurée, démo convaincante, Q&A préparée

---

## 💡 Checklist finale

```
3 mois avant:
- [ ] Sujet validé
- [ ] Cahier des charges rédigé
- [ ] Technologies choisies
- [ ] Repository GitHub créé

2 mois avant:
- [ ] Architecture définie
- [ ] Backend 80% terminé
- [ ] Frontend démarré
- [ ] Tests unitaires en place

1 mois avant:
- [ ] Application fonctionnelle end-to-end
- [ ] Tests E2E passants
- [ ] Déploiement réalisé
- [ ] Documentation démarrée

2 semaines avant:
- [ ] Rapport technique finalisé
- [ ] Slides présentation créées
- [ ] Démo testée 5+ fois
- [ ] Questions anticipées préparées

1 semaine avant:
- [ ] Répétition complète présentation
- [ ] Vidéo backup enregistrée
- [ ] Documents imprimés
- [ ] Repos et confiance!
```

---

[⬅️ Partie VI](./README.md) | [➡️ Chapitre 16](./chapitre-16-etude-de-cas-complete.md)

