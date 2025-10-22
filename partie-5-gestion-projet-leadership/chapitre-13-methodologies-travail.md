# Chapitre 13 : Méthodologies de travail

> **Objectif:** Maîtriser les méthodologies Agile, la planification de projets, l'estimation et les outils collaboratifs pour une gestion efficace des développements.

---

## 📋 Table des matières

1. [Agile, Scrum, Kanban, DevOps](#131-agile-scrum-kanban-devops)
2. [Planification et priorisation des tâches](#132-planification-et-priorisation-des-tâches)
3. [Estimation des charges et suivi d'avancement](#133-estimation-des-charges-et-suivi-davancement)
4. [Outils collaboratifs et communication d'équipe](#134-outils-collaboratifs-et-communication-déquipe)

---

## 13.1 Agile, Scrum, Kanban, DevOps

### 13.1.1 Manifeste Agile

**4 valeurs:**
1. **Individus et interactions** > Processus et outils
2. **Logiciel fonctionnel** > Documentation exhaustive
3. **Collaboration avec le client** > Négociation contractuelle
4. **Adaptation au changement** > Suivi d'un plan

**12 principes:** Satisfaction client, changements bienvenus, livraisons fréquentes, collaboration quotidienne, motivation, conversation face-à-face, logiciel fonctionnel comme mesure de progrès, rythme soutenable, excellence technique, simplicité, auto-organisation, rétrospectives régulières.

### 13.1.2 Scrum

**Rôles:**
- **Product Owner:** Maximise la valeur produit, gère le backlog
- **Scrum Master:** Facilite le processus, élimine les obstacles
- **Development Team:** Équipe auto-organisée (5-9 personnes)

**Artefacts:**
- **Product Backlog:** Liste priorisée de toutes les user stories
- **Sprint Backlog:** User stories sélectionnées pour le sprint
- **Increment:** Produit potentiellement livrable à la fin du sprint

**Événements:**

```
Sprint (2-4 semaines)
├─ Sprint Planning (début)
│  └─ Quoi: Sélection user stories
│  └─ Comment: Découpage en tâches
│
├─ Daily Scrum (quotidien, 15 min)
│  └─ Hier: Ce qui a été fait
│  └─ Aujourd'hui: Ce qui sera fait
│  └─ Obstacles: Problèmes rencontrés
│
├─ Sprint Review (fin)
│  └─ Démonstration de l'incrément
│  └─ Feedback des stakeholders
│
└─ Sprint Retrospective (fin)
   └─ Ce qui a bien fonctionné
   └─ Ce qui peut être amélioré
   └─ Actions d'amélioration
```

**Exemple de Sprint:**

```
Sprint 12: Du 21/10 au 03/11 (2 semaines)

Sprint Goal: Implémenter système de notifications

Product Backlog (priorisé):
1. US-045: Notifications email (priorité haute)
2. US-046: Notifications push mobile (priorité haute)
3. US-047: Centre de notifications in-app (priorité moyenne)
4. US-048: Préférences notifications (priorité basse)

Sprint Planning:
- Capacité équipe: 80 story points
- Stories sélectionnées: US-045 (21 pts), US-046 (34 pts), US-047 (21 pts)
- Total: 76 story points

Daily Scrum (exemple jour 5):
- John: Hier terminé API notifications, aujourd'hui intégration mobile, pas d'obstacles
- Sarah: Hier UI centre notifications, aujourd'hui tests, obstacle: environnement test down
- Mike: Hier fix bugs email, aujourd'hui code review, pas d'obstacles

Sprint Review:
- Démo: Notifications email + push fonctionnelles
- Feedback: Centre notifications apprécié, demande ajout filtre par type
- US-047 ajusté, US-048 passera au prochain sprint

Sprint Retrospective:
✅ Bien: Communication améliorée, pair programming efficace
❌ À améliorer: Tests automatisés incomplets, meetings trop longs
→ Actions: Augmenter coverage à 85%, limiter daily à 15min strict
```

### 13.1.3 Kanban

**Principes:**
1. Visualiser le flux de travail
2. Limiter le travail en cours (WIP limits)
3. Gérer le flux
4. Rendre les règles explicites
5. Améliorer collaborativement

**Tableau Kanban:**

```
┌────────┬─────────┬───────────┬─────────┬──────┐
│ Backlog│  To Do  │ In Progress│ Review  │ Done │
│        │  (WIP:5)│  (WIP:3)   │ (WIP:2) │      │
├────────┼─────────┼────────────┼─────────┼──────┤
│ US-051 │ US-048  │   US-045   │  US-043 │US-040│
│ US-052 │ US-049  │   US-046   │  US-044 │US-041│
│ US-053 │ US-050  │   US-047   │         │US-042│
│  ...   │         │            │         │  ... │
└────────┴─────────┴────────────┴─────────┴──────┘

WIP Limits: 
- To Do: 5 max (évite surcharge planification)
- In Progress: 3 max (focus, évite multitasking)
- Review: 2 max (revues rapides)
```

**Métriques Kanban:**
- **Lead Time:** Temps total de backlog à done
- **Cycle Time:** Temps de "to do" à done
- **Throughput:** Nombre de tâches complétées par période
- **WIP:** Nombre de tâches en cours

### 13.1.4 DevOps

**Culture DevOps:**
- **Collaboration** Dev + Ops
- **Automatisation** (CI/CD, tests, déploiements)
- **Mesure** (monitoring, métriques)
- **Partage** (connaissances, responsabilités)

**Pratiques:**

```
Code → Build → Test → Release → Deploy → Operate → Monitor
  ↑                                                      ↓
  └──────────────── Feedback Loop ──────────────────────┘

1. Version Control (Git)
2. Continuous Integration (Jenkins, GitHub Actions)
3. Continuous Delivery/Deployment
4. Infrastructure as Code (Terraform, Ansible)
5. Monitoring & Logging (Prometheus, ELK)
6. Microservices & Containers (Docker, Kubernetes)
```

---

## 13.2 Planification et priorisation des tâches

### 13.2.1 MoSCoW

```
Must have     - Essentiel, blocant si absent
Should have   - Important mais non bloquant
Could have    - Souhaitable si temps/ressources
Won't have    - Explicitement exclu de cette release
```

**Exemple:**

```
Release 2.0 - Système de paiement

Must have:
- Intégration Stripe
- Validation carte de crédit
- Confirmation de paiement
- Sécurité PCI-DSS

Should have:
- Support PayPal
- Sauvegarde moyens de paiement
- Facturation automatique

Could have:
- Support crypto-monnaies
- Paiement en plusieurs fois
- Wallet virtuel

Won't have (cette version):
- Paiement par virement bancaire
- Intégration comptabilité ERP
```

### 13.2.2 Value vs Effort Matrix

```
         Valeur
           ↑
    High   │  Quick Wins  │  Major Projects
           │    ★★★★★    │     ★★★★
           │              │
───────────┼──────────────┼────────────────→ Effort
           │              │
    Low    │  Fill-ins    │  Time Sinks
           │    ★★        │      ★
```

### 13.2.3 Roadmap produit

```
Q4 2024              Q1 2025              Q2 2025
────────────────────────────────────────────────────────
│ v2.0            │ v2.1            │ v2.2            │
│ Paiements       │ Mobile app      │ Analytics       │
│ - Stripe        │ - iOS/Android   │ - Dashboard     │
│ - PayPal        │ - Push notifs   │ - Rapports      │
│                 │                 │ - Exports       │
│                 │                 │                 │
│ v2.0.1 (hotfix) │ v2.1.1 (hotfix) │                 │
└─────────────────┴─────────────────┴─────────────────┘

Légende:
★ Must have    ◆ Should have    ○ Could have
```

---

## 13.3 Estimation des charges et suivi d'avancement

### 13.3.1 Story Points (Planning Poker)

**Suite de Fibonacci:** 0, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89

```
1 point  = Très simple (< 1 heure)
2 points = Simple (1-2 heures)
3 points = Moyen (demi-journée)
5 points = Complexe (1 jour)
8 points = Très complexe (2-3 jours)
13+ points = Epic, à découper

Exemple:
US-045: Ajouter bouton "like" → 2 points
US-046: Implémenter système de notifications → 13 points (epic!)
US-047: Fix bug affichage → 1 point
```

**Planning Poker:**
1. Product Owner lit la user story
2. Équipe pose des questions
3. Chacun choisit secrètement une carte (story points)
4. Révélation simultanée
5. Si consensus → estimation adoptée
6. Sinon → discussion, puis nouveau vote

### 13.3.2 Vélocité

```
Vélocité = Story points complétés par sprint

Sprint 1: 45 points
Sprint 2: 52 points
Sprint 3: 48 points
Vélocité moyenne: 48 points/sprint

Prévision:
Backlog restant: 240 points
Nombre de sprints: 240 / 48 = 5 sprints (~10 semaines)
```

### 13.3.3 Burn-down Chart

```
Story Points
    100│●
       │ ●
     80│   ●
       │     ●
     60│       ●
       │         ●
     40│           ●    Idéal: ligne droite
       │             ●●  Réel: ligne ajustée
     20│               ●
       │                 ●
      0└────────────────────●─→ Jours
        0  2  4  6  8 10 12 14

Interprétation:
- Ligne au-dessus de l'idéal: retard
- Ligne en-dessous: avance
- Pente aplatie: obstacles
```

---

## 13.4 Outils collaboratifs et communication d'équipe

### 13.4.1 Jira

```
Hiérarchie:
Epic
 └─ Story
     └─ Sub-task

Exemple:
Epic: Système de notifications
├─ Story: Notifications email
│  ├─ Sub-task: Design template email
│  ├─ Sub-task: Implémenter service envoi
│  └─ Sub-task: Tests A/B templates
├─ Story: Notifications push
│  └─ Sub-task: Intégration Firebase
└─ Story: Centre notifications
   └─ Sub-task: UI composants
```

**Workflow Jira:**

```
To Do → In Progress → Code Review → Testing → Done
  ↓                                              ↑
  └──────────────── Blocked ────────────────────┘
```

### 13.4.2 Confluence (Documentation)

**Structure type:**

```
Espace: Projet E-commerce
├─ Home
├─ Roadmap
├─ Architecture
│  ├─ Vue d'ensemble
│  ├─ Diagrammes C4
│  └─ ADRs
├─ API Documentation
├─ Runbooks
│  ├─ Déploiement production
│  ├─ Incident response
│  └─ Backup & restore
├─ Meeting Notes
│  ├─ Sprint Planning
│  ├─ Sprint Review
│  └─ Retrospectives
└─ Knowledge Base
   ├─ Onboarding
   ├─ Coding standards
   └─ Best practices
```

### 13.4.3 Communication asynchrone

**Slack/Teams - Bonnes pratiques:**

```
Channels:
#dev-general      → Discussions générales dev
#dev-frontend     → Frontend spécifique
#dev-backend      → Backend spécifique
#ops-incidents    → Incidents production
#announcements    → Annonces importantes
#random           → Off-topic

Conventions:
- Thread pour discussions longues
- @channel seulement si urgent
- Statut: 🟢 Disponible, 🔴 Occupé, 🟡 Absent
- Réactions pour feedback rapide (👍 vu, ✅ fait, ❓ question)
```

**Réunions efficaces:**

```
Daily Standup (15 min max):
- Synchrone ou asynchrone (Slack)
- 3 questions: Hier? Aujourd'hui? Obstacles?
- Pas de résolution de problèmes → parking lot

Sprint Planning (2h pour 2 semaines):
- Agenda partagé à l'avance
- Backlog raffiné au préalable
- Timeboxé strictement

Retrospective (1h):
- Format varié (Start/Stop/Continue, 4Ls, etc.)
- Actions concrètes, assignées, suivies
- Safe space, pas de blame
```

---

## 📝 Résumé

✅ **Agile:** Valeurs et principes pour adaptabilité

✅ **Scrum:** Framework avec sprints, roles et événements

✅ **Kanban:** Flux visualisé avec WIP limits

✅ **DevOps:** Culture collaboration + automatisation

✅ **Priorisation:** MoSCoW, Value/Effort matrix

✅ **Estimation:** Story points, planning poker, vélocité

✅ **Outils:** Jira, Confluence, Slack pour collaboration

---

## 💡 Exercices

1. Simuler un Sprint Planning pour un projet fictif
2. Créer un tableau Kanban et gérer le flux pendant 2 semaines
3. Estimer un backlog de 20 user stories en planning poker

---

[⬅️ Partie V](./README.md) | [➡️ Chapitre 14](./chapitre-14-leadership-technique.md)

