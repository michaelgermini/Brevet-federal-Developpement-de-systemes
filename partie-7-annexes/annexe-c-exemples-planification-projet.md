# Annexe C : Exemples de planification projet

Cette annexe propose des modèles et exemples concrets pour la planification, le suivi et la gestion de projets de développement.

---

## C.1 Work Breakdown Structure (WBS) détaillé

### Exemple: Application e-commerce

```
1.0 Analyse et conception
├── 1.1 Étude des besoins
│   ├── 1.1.1 Interviews stakeholders (4h)
│   ├── 1.1.2 Analyse concurrentielle (6h)
│   ├── 1.1.3 Rédaction cahier des charges (8h)
│   └── 1.1.4 Validation exigences (4h)
├── 1.2 Modélisation
│   ├── 1.2.1 Diagrammes UML (use cases, classes) (12h)
│   ├── 1.2.2 Modèle de données (8h)
│   └── 1.2.3 Architecture technique (8h)
└── 1.3 Prototypage
    ├── 1.3.1 Maquettes UI/UX (16h)
    └── 1.3.2 Prototype interactif (12h)

2.0 Développement backend
├── 2.1 Setup infrastructure
│   ├── 2.1.1 Configuration serveur (4h)
│   ├── 2.1.2 Base de données (6h)
│   └── 2.1.3 CI/CD pipeline (8h)
├── 2.2 API REST
│   ├── 2.2.1 Endpoints utilisateurs (16h)
│   ├── 2.2.2 Endpoints produits (12h)
│   ├── 2.2.3 Endpoints commandes (20h)
│   └── 2.2.4 Authentification JWT (8h)
├── 2.3 Logique métier
│   ├── 2.3.1 Gestion utilisateurs (12h)
│   ├── 2.3.2 Catalogue produits (10h)
│   ├── 2.3.3 Panier d'achat (8h)
│   ├── 2.3.4 Traitement commandes (16h)
│   └── 2.3.5 Intégration paiement (12h)
└── 2.4 Tests backend
    ├── 2.4.1 Tests unitaires (24h)
    ├── 2.4.2 Tests d'intégration (12h)
    └── 2.4.3 Tests API (8h)

3.0 Développement frontend
├── 3.1 Configuration projet
│   ├── 3.1.1 Setup React/TypeScript (4h)
│   ├── 3.1.2 Configuration build (4h)
│   └── 3.1.3 Routing (4h)
├── 3.2 Composants UI
│   ├── 3.2.1 Header/Navigation (6h)
│   ├── 3.2.2 Pages produit (12h)
│   ├── 3.2.3 Panier (8h)
│   ├── 3.2.4 Formulaire commande (10h)
│   └── 3.2.5 Dashboard utilisateur (8h)
├── 3.3 Intégration API
│   ├── 3.3.1 Services HTTP (8h)
│   ├── 3.3.2 Gestion erreurs (4h)
│   ├── 3.3.3 Cache local (6h)
│   └── 3.3.4 State management (12h)
└── 3.4 Tests frontend
    ├── 3.4.1 Tests composants (16h)
    ├── 3.4.2 Tests intégration (8h)
    └── 3.4.3 Tests E2E (12h)

4.0 Qualité et sécurité
├── 4.1 Tests fonctionnels
│   ├── 4.1.1 Tests manuels (16h)
│   ├── 4.1.2 Tests automatisés (12h)
│   └── 4.1.3 Tests de régression (8h)
├── 4.2 Sécurité
│   ├── 4.2.1 Audit sécurité (8h)
│   ├── 4.2.2 Chiffrement données (6h)
│   └── 4.2.3 Validation entrées (8h)
└── 4.3 Performance
    ├── 4.3.1 Optimisation DB (8h)
    ├── 4.3.2 Cache (6h)
    └── 4.3.3 Tests de charge (12h)

5.0 Déploiement et documentation
├── 5.1 Infrastructure
│   ├── 5.1.1 Serveur production (8h)
│   ├── 5.1.2 Configuration SSL (4h)
│   └── 5.1.3 Monitoring (8h)
├── 5.2 Déploiement
│   ├── 5.2.1 Scripts déploiement (6h)
│   ├── 5.2.2 Tests production (8h)
│   └── 5.2.3 Rollback plan (4h)
└── 5.3 Documentation
    ├── 5.3.1 Guide utilisateur (12h)
    ├── 5.3.2 Documentation technique (16h)
    └── 5.3.3 Formation équipe (4h)

6.0 Projet de diplôme
├── 6.1 Préparation soutenance
│   ├── 6.1.1 Rapport final (40h)
│   ├── 6.1.2 Slides présentation (8h)
│   └── 6.1.3 Répétitions (6h)
└── 6.2 Soutenance
    ├── 6.2.1 Présentation (2h)
    ├── 6.2.2 Démonstration (1h)
    └── 6.2.3 Questions jury (1h)

Total estimé: ~600 heures (15 semaines à temps plein)
```

---

## C.2 Planning Gantt avec jalons

### Vue d'ensemble (15 semaines)

```
Semaine:   1    2    3    4    5    6    7    8    9   10   11   12   13   14   15
         ┌────┬────┬────┬────┬────┬────┬────┬────┬────┬────┬────┬────┬────┬────┬────┐
Analyse: ████████████████████████████████████████.......................................
         └──────────────────────────────────────┘
Concep.: ......................................███████████████████████████████████████
         ┌──────────────────────────────────────┼─────────────────────────────────────┘
Backend: ........................███████████████.......................................
         ┌──────────────────────┼──────────────┘
Frontend: ......................███████████████.......................................
         ┌─────────────────────┼──────────────┘
Tests:   .....................██████████████████.......................................
         ┌────────────────────┼───────────────┘
Sécur.:  ....................███████████████..........................................
         ┌───────────────────┼─────────────┘
Déploi.: ...................███████████████..........................................
         ┌──────────────────┼─────────────┘
Doc:     ..................██████████████████..........................................
         ┌─────────────────┼───────────────┘
Diplôme: ................███████████████████..........................................
         ┌───────────────┼─────────────────┘

Jalons:         J1         J2         J3         J4         J5         J6         J7
```

### Détail par semaine

```
Semaine 1-2: Analyse (40h)
├── Jour 1-2: Interviews utilisateurs
├── Jour 3-4: Analyse besoins fonctionnels
├── Jour 5-6: Analyse besoins non fonctionnels
├── Jour 7-8: Rédaction cahier des charges
└── Jour 9-10: Validation avec stakeholders

Jalon J1: Cahier des charges approuvé

Semaine 3-4: Conception (60h)
├── Jour 11-15: Diagrammes UML (use cases, classes, séquences)
├── Jour 16-18: Modèle de données ERD
├── Jour 19-22: Architecture technique (diagrammes C4)
├── Jour 23-25: Choix technologiques (ADR)
├── Jour 26-28: Prototypes UI/UX
└── Jour 29-30: Planning détaillé

Jalon J2: Architecture validée

Semaine 5-7: Backend (120h)
├── Jour 31-35: Setup infrastructure (Docker, DB, CI/CD)
├── Jour 36-45: API REST utilisateurs
├── Jour 46-55: API REST produits/commandes
├── Jour 56-60: Authentification et sécurité
├── Jour 61-70: Tests unitaires et d'intégration
└── Jour 71-77: Optimisations et documentation

Jalon J3: Backend fonctionnel

Semaine 8-10: Frontend (120h)
├── Jour 78-82: Setup React/TypeScript
├── Jour 83-95: Composants UI principaux
├── Jour 96-105: Intégration API
├── Jour 106-110: State management et routing
├── Jour 111-120: Tests et responsive design
└── Jour 121-125: Optimisations UX

Jalon J4: MVP complet

Semaine 11-12: Tests & Qualité (80h)
├── Jour 126-135: Tests E2E et fonctionnels
├── Jour 136-140: Tests de performance
├── Jour 141-145: Audit sécurité (OWASP)
├── Jour 146-150: Corrections bugs
└── Jour 151-154: Tests de régression

Jalon J5: Prêt pour production

Semaine 13-14: Déploiement (60h)
├── Jour 155-160: Configuration production
├── Jour 161-165: Scripts déploiement automatisé
├── Jour 166-170: Tests en environnement réel
├── Jour 171-175: Monitoring et logging
└── Jour 176-180: Documentation exploitation

Jalon J6: Déploiement réussi

Semaine 15: Diplôme (40h)
├── Jour 181-190: Rédaction rapport final
├── Jour 191-195: Préparation slides
├── Jour 196-198: Répétitions présentation
└── Jour 199-200: Finalisations et backup

Jalon J7: Soutenance réussie
```

---

## C.3 Définition of Done (DoD)

### Niveau User Story
```
✅ Code écrit et commité
✅ Tests unitaires écrits et passant
✅ Code review effectuée et approuvée
✅ Tests d'intégration passant
✅ Documentation mise à jour
✅ Démonstration fonctionnelle faite
✅ Product Owner satisfait
✅ Tests d'acceptation passant
```

### Niveau Sprint
```
✅ Toutes les user stories terminées selon DoD
✅ Tests de régression passant
✅ Code déployé en staging
✅ Documentation sprint à jour
✅ Démonstration sprint faite
✅ Métriques de qualité atteintes (>80% coverage)
✅ Dette technique maîtrisée
✅ Retrospective effectuée
```

### Niveau Release
```
✅ Sprint goals atteints
✅ Tests E2E passant
✅ Tests de performance validés
✅ Sécurité auditée (OWASP Top 10)
✅ Documentation complète
✅ Formation utilisateurs prête
✅ Rollback plan documenté
✅ Monitoring en place
```

---

## C.4 Fiches qualité (Quality Gates)

### Quality Gate 1: Commit
```
Avant chaque commit:
□ Code compile sans erreurs
□ Tests unitaires passent localement
□ Linting passe (ESLint, Prettier)
□ Pas de console.log ou TODOs temporaires
□ Commit message conventionnel (feat/fix/docs)
□ Code review si modification critique
```

### Quality Gate 2: Pull Request
```
Avant merge:
□ Tests automatisés passent (CI)
□ Code coverage > 80%
□ Code review effectuée (au moins 1 approbation)
□ Pas de conflits avec main
□ Documentation mise à jour
□ Impact sécurité vérifié
□ Performance testée
□ Branches feature supprimées après merge
```

### Quality Gate 3: Sprint Review
```
Avant fin de sprint:
□ Toutes user stories avec DoD respecté
□ Tests d'intégration passant
□ Bugs critiques corrigés
□ Performance acceptable
□ Documentation fonctionnelle à jour
□ Product Owner satisfait
□ Métriques sprint positives
```

### Quality Gate 4: Release
```
Avant déploiement production:
□ Tests E2E complets passant
□ Tests de charge validés
□ Audit sécurité passé
□ Backup et rollback testés
□ Runbook déploiement validé
□ Communication utilisateurs prête
□ Monitoring et alerting configurés
□ Support équipe disponible
```

---

## C.5 Budget et ressources

### Estimation budgétaire (CHF)

```
Ressources humaines:
├── Développeur senior (15 semaines × 40h × 120 CHF/h): 72,000
├── Développeur junior (15 semaines × 40h × 80 CHF/h): 48,000
├── QA/Testeur (8 semaines × 40h × 90 CHF/h): 28,800
└── Product Owner (6 semaines × 20h × 100 CHF/h): 12,000
Sous-total RH: 160,800

Infrastructure:
├── Serveur développement (3 mois × 50 CHF/mois): 150
├── Serveur staging (3 mois × 100 CHF/mois): 300
├── Serveur production (3 mois × 200 CHF/mois): 600
├── Base de données cloud (3 mois × 150 CHF/mois): 450
└── Outils (GitHub Pro, monitoring): 300
Sous-total Infra: 1,800

Logiciels et licences:
├── IDE et outils développement: 500
├── Tests (licences commerciales si nécessaire): 1,000
├── Sécurité (outils audit): 800
└── Documentation (outils): 200
Sous-total Logiciels: 2,500

Formation et externalisation:
├── Formation équipe (2 jours × 5 personnes × 800 CHF): 8,000
├── Audit sécurité externe: 5,000
└── Support technique: 2,000
Sous-total Formation: 15,000

Contingence (15%): 27,000

TOTAL ESTIMÉ: 207,100 CHF
```

### Répartition par phase

```
Phase 1: Analyse & Conception (15%): 31,065 CHF
├── Ressources humaines: 24,000
├── Outils: 6,000
└── Formation: 1,065

Phase 2: Développement (50%): 103,550 CHF
├── Backend: 41,400
├── Frontend: 34,500
├── Tests: 20,700
└── Infrastructure: 6,950

Phase 3: Déploiement & Support (25%): 51,775 CHF
├── Infrastructure production: 12,000
├── Monitoring: 8,000
├── Support: 15,000
└── Documentation: 16,775

Phase 4: Projet diplôme (10%): 20,710 CHF
├── Rapport et préparation: 18,000
├── Soutenance: 2,710
└── Finalisations: 0

Contingence (15%): 27,000 CHF
```

---

## C.6 Suivi d'avancement (Dashboard)

### Métriques de suivi

```markdown
# Dashboard Projet - Semaine [X]

## 🎯 Objectifs Sprint
- [ ] US-045: Authentification utilisateur
- [ ] US-046: Catalogue produits
- [ ] US-047: Panier d'achat
- [X] US-048: Validation formulaires

## 📊 Métriques

### Vélocité
- Sprint actuel: 42 story points
- Sprint précédent: 38 story points
- Moyenne: 40 story points/sprint
- Tendance: 📈 +11%

### Qualité
- Tests passant: 94% (187/200)
- Coverage code: 82%
- Code smells: 12 (target: < 20)
- Security issues: 0

### Performance
- Build time: 4:32 min (target: < 5 min)
- Tests time: 8:15 min (target: < 10 min)
- Deployment time: 2:45 min (target: < 3 min)

## 🔴 Risques
1. **Délai serré US-045** - Risque: Moyen
   - Impact: Retard livraison
   - Mitigation: Focus équipe, heures supp. si nécessaire

2. **Dépendance API externe** - Risque: Faible
   - Impact: Fonctionnalité dégradée
   - Mitigation: Fallback local, cache

## ✅ Accomplissements
- ✅ Architecture backend finalisée
- ✅ 3 APIs REST implémentées
- ✅ Tests unitaires > 80% coverage
- ✅ Intégration CI/CD fonctionnelle

## 🚧 En cours
- 🔄 Développement interface panier
- 🔄 Tests d'intégration
- 🔄 Revue de sécurité

## 📅 Prochaines étapes
- Semaine X+1: Finalisation frontend
- Semaine X+2: Tests E2E
- Semaine X+3: Déploiement staging

## 👥 Équipe
- **Lead Dev:** Jean Dupont (100% dédié)
- **Dev Frontend:** Marie Martin (80% disponible)
- **Dev Backend:** Pierre Dubois (100% dédié)
- **QA:** Sophie Leroy (60% disponible)

## 💰 Budget
- Consommé: 145,200 CHF (70%)
- Restant: 61,900 CHF (30%)
- Écart: +2,100 CHF (sur budget)
- Prévisionnel fin: 201,100 CHF
```

### Burn-down Chart Template

```
Story Points Restants
     100 │         ●
         │        ● ●
      80 │       ●   ●
         │      ●     ●
      60 │     ●       ●
         │    ●         ●
      40 │   ●           ●
         │  ●             ●
      20 │ ●               ●
         │●                 ●
       0 └───────────────────●──→ Jours
         0  2  4  6  8 10 12 14

Légende:
● Idéal (ligne droite)
● Réel (ligne ajustée)
```

---

## C.7 Plan de communication

### Communication interne

#### Daily Standup (quotidien, 15 min)
```
Format: Synchrone (9h00-9h15)
Participants: Équipe développement
Contenu:
- Hier: Accomplissements
- Aujourd'hui: Objectifs
- Obstacles: Blocages
Outil: Microsoft Teams
```

#### Weekly Review (hebdomadaire, 45 min)
```
Format: Synchrone (vendredi 16h00-16h45)
Participants: Équipe + Product Owner
Contenu:
- Démonstration avancement
- Revue métriques
- Ajustements planning
Outil: Zoom + Jira
```

#### Monthly Retrospective (mensuel, 1h)
```
Format: Synchrone (dernier vendredi du mois)
Participants: Équipe complète
Contenu:
- Ce qui a bien fonctionné
- Ce qui peut être amélioré
- Actions d'amélioration
Outil: Miro (board collaboratif)
```

### Communication externe

#### Rapport Client (bi-hebdomadaire)
```
Destinataires: Client, direction
Format: Email + PDF
Contenu:
- Avancement général
- Jalons atteints/à venir
- Risques identifiés
- Métriques clés
Fréquence: Mardi et vendredi
```

#### Newsletter Équipe (hebdomadaire)
```
Destinataires: Équipe élargie
Format: Email HTML
Contenu:
- Highlights semaine
- Annonces importantes
- Événements à venir
- Reconnaissances individuelles
Fréquence: Vendredi après-midi
```

#### Escalade Problèmes
```
Niveau 1 (Équipe): Résolution immédiate
Niveau 2 (Lead): Escalade < 4h si non résolu
Niveau 3 (Direction): Escalade < 24h si impact business
Outil: Jira + Slack
```

---

## C.8 Plan de gestion des risques

### Matrice de risques

| Risque | Probabilité | Impact | Score | Mitigation | Owner |
|--------|-------------|--------|-------|------------|-------|
| Retard livraison | Moyenne | Élevé | 12 | Buffer 20% planning | Lead Dev |
| Départ équipe | Faible | Élevé | 8 | Documentation complète | Manager |
| Bug sécurité critique | Faible | Critique | 9 | Audit régulier | Security Lead |
| Changement exigences | Élevée | Moyenne | 10 | Process change request | Product Owner |
| Problème performance | Moyenne | Moyenne | 6 | Tests charge réguliers | DevOps |
| Dépendance externe down | Moyenne | Faible | 4 | Monitoring + fallback | DevOps |

### Plan de contingence

#### Scénario 1: Retard développeur
```
Déclencheur: Maladie > 1 semaine
Actions:
1. Redistribution tâches équipe
2. Recrutement temporaire si nécessaire
3. Focus fonctionnalités critiques
4. Communication client transparente
Responsable: Lead Developer
```

#### Scénario 2: Changement scope important
```
Déclencheur: Nouvelle exigence critique
Actions:
1. Évaluation impact (effort, délai)
2. Négociation avec client (trade-offs)
3. Ajustement planning et budget
4. Documentation changement (ADR)
Responsable: Product Owner
```

#### Scénario 3: Incident sécurité
```
Déclencheur: Vulnérabilité découverte
Actions:
1. Isolation système compromis
2. Investigation (forensics)
3. Correction vulnérabilité
4. Communication stakeholders
5. Post-mortem et améliorations
Responsable: Security Lead
```

---

## C.9 Modèle de rapport d'avancement

```markdown
# Rapport d'Avancement - Projet [Nom]

**Période:** Du YYYY-MM-DD au YYYY-MM-DD
**Version:** v[X.Y]

## 📊 Synthèse

### Status général
- **Phase actuelle:** [Développement Backend]
- **Avancement global:** 68% (408/600 heures)
- **État:** [🟢 Dans les temps | 🟡 Risque retard | 🔴 Retard]

### Prochain jalon
- **Jalon:** [Backend fonctionnel]
- **Échéance:** [YYYY-MM-DD]
- **Avancement:** 85% (102/120 heures)
- **Risques:** [Aucun | Mineur | Majeur]

## 🎯 Accomplissements

### Cette période
- ✅ [Accomplissement 1]
- ✅ [Accomplissement 2]
- ✅ [Accomplissement 3]

### Métriques
- **Story points complétés:** 42 (vs 38 prévu)
- **Tests passant:** 94%
- **Coverage code:** 82%
- **Déploiement réussi:** 3 (staging)

## 🚧 En cours

### Tâches prioritaires
1. **[US-045]** Intégration API paiement - 80% (2 jours restants)
2. **[US-046]** Tests E2E commandes - 60% (3 jours restants)
3. **[US-047]** Documentation API - 40% (1 jour restant)

### Blocages
- **Blocage 1:** [Description] - [Solution en cours]
- **Blocage 2:** [Description] - [Attente réponse fournisseur]

## 🔮 Prévisions

### Semaine prochaine
- **Objectif:** Finalisation backend + début frontend
- **Risques:** [Dépendance API tierce]
- **Actions:** [Réunion clarification exigences]

### Ajustements planning
- **Phase backend:** +2 jours (optimisations performance)
- **Phase tests:** -1 jour (automatisation réussie)
- **Phase frontend:** Début décalé de 3 jours

## 👥 Ressources

### Équipe
- **Jean Dupont** (Lead Dev): 100% - Excellente performance
- **Marie Martin** (Frontend): 80% - Disponibilité réduite cette semaine
- **Pierre Dubois** (Backend): 100% - Focus sur optimisation
- **Sophie Leroy** (QA): 60% - Support testing

### Budget
- **Consommé:** 145,200 CHF (70%)
- **Restant:** 61,900 CHF (30%)
- **Écart:** +2,100 CHF (sur budget)
- **Prévisionnel fin:** 201,100 CHF

## ⚠️ Risques et actions

### Risques ouverts
1. **Risque 1** - Probabilité: [Moyenne] - Impact: [Élevé]
   - Status: [Suivi] - Actions: [En cours]

2. **Risque 2** - Probabilité: [Faible] - Impact: [Moyen]
   - Status: [Mitigé] - Actions: [Complétées]

### Nouveaux risques
- **Risque X:** [Description] - Mitigation: [Plan]

## 📋 Actions requises

### Immédiat (< 24h)
- [ ] [Action urgente 1]
- [ ] [Action urgente 2]

### Court terme (< 1 semaine)
- [ ] [Action 1]
- [ ] [Action 2]

### Support demandé
- [ ] [Demande 1] - À: [Personne/Équipe]
- [ ] [Demande 2] - À: [Personne/Équipe]

## 📞 Prochaines communications

- **Daily standup:** Quotidien 9h00
- **Weekly review:** Vendredi YYYY-MM-DD 16h00
- **Rapport client:** Mardi YYYY-MM-DD
- **Point direction:** [Si nécessaire]

## 📎 Annexes

1. [Burn-down chart actuel](burn-down-week-X.png)
2. [Matrice de traçabilité](traceability-matrix.xlsx)
3. [Rapport qualité](quality-report-week-X.pdf)
4. [Logs d'erreurs](error-logs-week-X.txt)

---

**Rapport établi par:** [Votre nom]
**Approuvé par:** [Manager/Lead]
**Date:** YYYY-MM-DD
```

---

[⬅️ Annexe B](./annexe-b-templates-tests-rapports.md) | [➡️ Annexe D](./annexe-d-liste-outils-recommandes.md)

