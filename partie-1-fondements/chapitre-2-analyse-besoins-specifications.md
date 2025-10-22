# Chapitre 2 : Analyse des besoins et spécifications

> **Objectif:** Maîtriser les techniques de collecte, d'analyse et de formalisation des besoins utilisateurs, en utilisant UML et les méthodes d'analyse structurée.

---

## 📋 Table des matières

1. [Collecte et formalisation des besoins utilisateurs](#21-collecte-et-formalisation-des-besoins-utilisateurs)
2. [Diagrammes UML](#22-diagrammes-uml-cas-dutilisation-séquences-classes)
3. [Méthodes d'analyse fonctionnelle et technique](#23-méthodes-danalyse-fonctionnelle-et-technique)
4. [Documentation des exigences (SRS) et validation](#24-documentation-des-exigences-srs-et-validation)

---

## 2.1 Collecte et formalisation des besoins utilisateurs

### 2.1.1 Les user stories et le format As-a / I-want / So-that

**Format standard:**
```
En tant que [rôle]
Je veux [action/fonctionnalité]
Afin de [bénéfice/valeur métier]
```

**Exemples:**
```
US-001: Inscription utilisateur
En tant que visiteur
Je veux créer un compte avec mon email
Afin de pouvoir accéder aux fonctionnalités réservées aux membres

US-002: Réinitialisation mot de passe
En tant qu'utilisateur inscrit
Je veux réinitialiser mon mot de passe par email
Afin de pouvoir me reconnecter si je l'oublie
```

**Critères INVEST pour une bonne user story:**
- **I**ndependent: Indépendante des autres stories
- **N**egotiable: Peut être discutée et affinée
- **V**aluable: Apporte de la valeur à l'utilisateur
- **E**stimable: Peut être estimée en effort
- **S**mall: Suffisamment petite (1-3 jours de dev)
- **T**estable: Peut être validée par des tests

### 2.1.2 Critères d'acceptation

**Format Given-When-Then (Gherkin):**
```gherkin
User Story: Connexion utilisateur

Critères d'acceptation:

Scénario 1: Connexion réussie avec identifiants valides
  Étant donné un utilisateur inscrit avec email "user@example.com" et mot de passe "Pass123!"
  Quand il saisit ses identifiants corrects
  Alors il est redirigé vers son tableau de bord
  Et un message de bienvenue s'affiche

Scénario 2: Connexion échouée avec mot de passe incorrect
  Étant donné un utilisateur inscrit
  Quand il saisit un mot de passe incorrect
  Alors un message d'erreur s'affiche
  Et il reste sur la page de connexion
  Et après 5 tentatives, le compte est temporairement bloqué

Scénario 3: Connexion avec email inexistant
  Étant donné un email non enregistré
  Quand l'utilisateur tente de se connecter
  Alors un message générique s'affiche (pour raisons de sécurité)
```

---

## 2.2 Diagrammes UML (cas d'utilisation, séquences, classes)

### 2.2.1 Diagramme de cas d'utilisation

**Éléments:**
- **Acteurs:** Utilisateurs ou systèmes externes
- **Cas d'utilisation:** Fonctionnalités du système
- **Relations:** Include, Extend, Généralisation

**Exemple: Système de gestion de bibliothèque**

```
                    ┌─────────────────────────────────────┐
                    │   Système Bibliothèque             │
                    │                                     │
┌──────────┐        │  ┌──────────────────┐             │
│          │        │  │  Rechercher      │             │
│ Visiteur │───────────│  un livre        │             │
│          │        │  └──────────────────┘             │
└──────────┘        │           │                        │
      △             │           │ <<include>>            │
      │             │           ▼                        │
      │             │  ┌──────────────────┐             │
┌──────────┐        │  │  Consulter       │             │
│          │        │  │  catalogue       │             │
│ Adhérent │───────────│                  │             │
│          │        │  └──────────────────┘             │
└──────────┘        │                                    │
      │             │  ┌──────────────────┐             │
      │             │  │  Emprunter       │             │
      │             ────│  un livre        │             │
      │             │  └──────────────────┘             │
      │             │           │                        │
      │             │           │ <<extend>>             │
      │             │           ▼                        │
      │             │  ┌──────────────────┐             │
      │             │  │  Prolonger       │             │
      │             │  │  l'emprunt       │             │
      │             │  └──────────────────┘             │
      │             │                                    │
      │             │  ┌──────────────────┐             │
      │             │  │  Retourner       │             │
      │             ────│  un livre        │             │
      │             │  └──────────────────┘             │
      │             │                                    │
┌──────────┐        │  ┌──────────────────┐             │
│          │        │  │  Gérer           │             │
│Bibliothé-│───────────│  les emprunts    │             │
│ caire    │        │  └──────────────────┘             │
└──────────┘        │                                    │
                    │  ┌──────────────────┐             │
                    │  │  Gérer le        │             │
                    ────│  catalogue       │             │
                    │  └──────────────────┘             │
                    │                                    │
                    └─────────────────────────────────────┘
```

**Relations:**
- **Include:** Comportement systématiquement inclus (ex: authentification avant emprunt)
- **Extend:** Comportement optionnel (ex: prolongation d'emprunt)
- **Généralisation:** Héritage entre acteurs (Adhérent hérite de Visiteur)

### 2.2.2 Diagramme de séquence

Modélise les interactions entre objets dans le temps.

**Exemple: Processus d'emprunt de livre**

```
Adhérent    Interface    EmpruntService    LivreRepository    EmpruntRepository
   │            │               │                  │                   │
   │─emprunter()│               │                  │                   │
   │────────────►               │                  │                   │
   │            │───emprunter() │                  │                   │
   │            │───────────────►                  │                   │
   │            │               │──getLivre(id)────►                   │
   │            │               │                  │                   │
   │            │               │◄─────livre───────│                   │
   │            │               │                  │                   │
   │            │               │──vérifierDisponibilité()              │
   │            │               │                  │                   │
   │            │               │[si disponible]   │                   │
   │            │               │                  │                   │
   │            │               │──créerEmprunt()──────────────────────►
   │            │               │                  │                   │
   │            │               │◄─────emprunt─────────────────────────│
   │            │               │                  │                   │
   │            │◄──empruntDTO──│                  │                   │
   │◄─confirmation─             │                  │                   │
   │            │               │                  │                   │
```

**Éléments clés:**
- **Ligne de vie:** Existence d'un objet dans le temps
- **Messages synchrones:** Flèche pleine (appel bloquant)
- **Messages asynchrones:** Flèche ouverte (non bloquant)
- **Fragments:** alt (alternative), loop (boucle), opt (optionnel)

### 2.2.3 Diagramme de classes

Représente la structure statique du système.

**Exemple: Modèle de domaine bibliothèque**

```
┌─────────────────────────────┐
│        Adherent             │
├─────────────────────────────┤
│ - id: Long                  │
│ - nom: String               │
│ - email: String             │
│ - dateInscription: Date     │
│ - fraisImpayes: BigDecimal  │
├─────────────────────────────┤
│ + emprunter(livre): Emprunt │
│ + peutEmprunter(): boolean  │
│ + payerFrais(montant): void │
└──────────────┬──────────────┘
               │ 1
               │ possède
               │ *
         ┌─────▼──────────────────────┐
         │       Emprunt              │
         ├────────────────────────────┤
         │ - id: Long                 │
         │ - dateEmprunt: LocalDate   │
         │ - dateRetourPrevue: Date   │
         │ - dateRetourEffective: Date│
         │ - statut: StatutEmprunt    │
         ├────────────────────────────┤
         │ + retourner(): void        │
         │ + prolonger(): void        │
         │ + calculerFraisRetard():..│
         │ + estEnRetard(): boolean   │
         └─────────┬──────────────────┘
                   │ *
                   │ concerne
                   │ 1
            ┌──────▼─────────────────┐
            │      Livre             │
            ├────────────────────────┤
            │ - id: Long             │
            │ - ISBN: String         │
            │ - titre: String        │
            │ - auteur: String       │
            │ - editeur: String      │
            │ - annee: int           │
            │ - categorie: Categorie │
            ├────────────────────────┤
            │ + estDisponible(): bool│
            └────────────────────────┘

┌────────────────┐
│<<enumeration>> │
│ StatutEmprunt  │
├────────────────┤
│ ACTIF          │
│ TERMINE        │
│ EN_RETARD      │
│ ANNULE         │
└────────────────┘
```

**Types de relations:**
- **Association:** Relation simple (ligne)
- **Agrégation:** Relation "a un" (diamant vide)
- **Composition:** Relation forte "compose de" (diamant plein)
- **Héritage:** Relation "est un" (flèche triangle)
- **Dépendance:** Utilisation ponctuelle (flèche pointillée)

---

## 2.3 Méthodes d'analyse fonctionnelle et technique

### 2.3.1 Méthode MERISE (Europe)

**Niveaux d'abstraction:**

1. **Niveau conceptuel (MCD - Modèle Conceptuel de Données)**
   - Entités, Attributs, Relations
   - Indépendant de l'implémentation

2. **Niveau logique (MLD - Modèle Logique de Données)**
   - Modèle relationnel
   - Tables, clés primaires, clés étrangères

3. **Niveau physique (MPD - Modèle Physique de Données)**
   - Implémentation concrète
   - Types de données spécifiques, index, partitions

**Exemple MCD:**
```
        1,n            0,n
ADHERENT ─────EMPRUNTE───── LIVRE
   │                         │
   │- numeroAdherent         │- ISBN
   │- nom                    │- titre
   │- prenom                 │- auteur
   │- email                  │
   │                         │
   └─ dateEmprunt ──────────┘
      dateRetour
```

**Transformation en MLD:**
```sql
ADHERENT (numeroAdherent, nom, prenom, email)
LIVRE (ISBN, titre, auteur, editeur)
EMPRUNT (idEmprunt, #numeroAdherent, #ISBN, dateEmprunt, dateRetour)
```

### 2.3.2 Analyse CRUD

**CRUD = Create, Read, Update, Delete**

Tableau de correspondance fonctionnalités/entités:

| Fonctionnalité | Adherent | Livre | Emprunt | Categorie |
|----------------|----------|-------|---------|-----------|
| Inscription | **C** | - | - | - |
| Consultation profil | R | - | - | - |
| Modification profil | U | - | - | - |
| Recherche livre | - | **R** | - | R |
| Emprunter | R | R | **C** | - |
| Retourner | R | R | **U** | - |
| Historique emprunts | R | R | **R** | - |
| Gérer catalogue | - | **CRUD** | - | CRUD |
| Statistiques | R | R | R | R |

**Utilité:**
- Vérifier la complétude fonctionnelle
- Identifier les opérations manquantes
- Estimer la complexité de développement

### 2.3.3 Analyse par Persona

**Définition:** Représentation fictive d'un utilisateur type basée sur des données réelles.

**Exemple:**

```
👤 Persona 1: Sophie, étudiante (22 ans)

Objectifs:
- Trouver rapidement des livres pour ses études
- Gérer ses emprunts en ligne
- Recevoir des rappels de retour

Frustrations:
- Perte de temps à chercher en bibliothèque
- Oubli des dates de retour → amendes
- Interface compliquée du système actuel

Comportement numérique:
- Utilise principalement son smartphone
- Active sur les réseaux sociaux
- Attend des réponses immédiates

Besoins fonctionnels:
- Application mobile responsive
- Recherche avancée (par cours, professeur)
- Notifications push pour les retours
- Prolongation en ligne

---

👤 Persona 2: Marc, bibliothécaire (45 ans)

Objectifs:
- Gérer efficacement les emprunts/retours
- Suivre l'état du catalogue
- Générer des rapports statistiques

Frustrations:
- Système lent lors des pics d'affluence
- Tâches répétitives manuelles
- Difficulté à suivre les retards

Compétences techniques:
- Maîtrise des outils bureautiques
- Peu à l'aise avec systèmes complexes
- Préfère les interfaces simples

Besoins fonctionnels:
- Interface de gestion rapide
- Scan de codes-barres
- Tableau de bord avec indicateurs
- Export Excel des données
- Processus optimisés pour gain de temps
```

**Carte d'empathie:**
```
┌────────────────────────────────────────┐
│  Ce qu'elle PENSE et RESSENT           │
│  "Je n'ai pas le temps d'aller         │
│   à la bibliothèque entre deux cours"  │
└────────────────────────────────────────┘

┌─────────────────┐    ┌─────────────────┐
│ Ce qu'elle VOIT │    │ Ce qu'elle      │
│                 │    │ ENTEND          │
│ - Amis utilisent│    │                 │
│   apps modernes │    │ - Profs parlent │
│ - Biblio bondée │    │   de ressources │
│ - Files d'attente│   │ - Recommandations│
└─────────────────┘    └─────────────────┘

┌─────────────────┐    ┌─────────────────┐
│ Ce qu'elle DIT  │    │ Ce qu'elle      │
│ et FAIT         │    │ FAIT            │
│                 │    │                 │
│ - "C'est trop   │    │ - Reporte emprunts│
│    compliqué"   │    │ - Préfère acheter│
│ - Utilise Google│    │   ou télécharger│
└─────────────────┘    └─────────────────┘

       PEURS                 GAINS
   - Payer amendes      - Gagner du temps
   - Perdre livres      - Accès facilité
   - Mauvaises notes    - Réussite études
```

---

## 2.4 Documentation des exigences (SRS) et validation

### 2.4.1 Techniques de validation des exigences

#### 1. Revue par les pairs (Peer Review)

**Processus:**
```
1. Préparation (1-2 jours avant)
   ├─ Distribution du SRS aux reviewers
   ├─ Checklist de revue fournie
   └─ Lecture individuelle

2. Réunion de revue (2-3 heures)
   ├─ Présentation par l'analyste (30 min)
   ├─ Questions et discussions (90 min)
   ├─ Identification des problèmes
   └─ Décisions: Accepté / Accepté avec modifications / Rejeté

3. Suivi
   ├─ Correction des points identifiés
   ├─ Nouvelle version du SRS
   └─ Validation finale
```

**Checklist de revue:**
```
□ Complétude: Toutes les fonctionnalités couvertes?
□ Cohérence: Pas de contradictions?
□ Clarté: Langage compréhensible pour toutes les parties?
□ Vérifiabilité: Chaque exigence est testable?
□ Traçabilité: Lien avec besoins métier?
□ Faisabilité technique: Réalisable avec ressources disponibles?
□ Priorisation: Criticité clairement définie?
□ Conformité: Respect des normes et réglementations?
```

#### 2. Prototypage et maquettage

**Types de prototypes:**

**Prototype basse fidélité (Wireframe):**
```
┌──────────────────────────────────────┐
│ [Logo]  Bibliothèque    [Profil▼]   │
├──────────────────────────────────────┤
│                                      │
│  ┌────────────────────────────────┐ │
│  │ 🔍 Rechercher un livre...      │ │
│  └────────────────────────────────┘ │
│                                      │
│  Catégories: [Sciences▼] [Tous▼]    │
│                                      │
│  ┌─────────────────────────────────┐│
│  │ [ ]  Clean Code - Robert Martin ││
│  │      ★★★★★ (234 avis)           ││
│  │      Disponible [Emprunter]     ││
│  ├─────────────────────────────────┤│
│  │ [ ]  Design Patterns - GoF      ││
│  │      ★★★★☆ (189 avis)           ││
│  │      Emprunté (retour: 15/11)   ││
│  └─────────────────────────────────┘│
│                                      │
│  [< Précédent]  [1][2][3]  [Suivant>]│
└──────────────────────────────────────┘
```

**Prototype haute fidélité:** Design avec couleurs, polices, images réelles

**Avantages:**
- Validation visuelle rapide
- Détection précoce des problèmes d'utilisabilité
- Base concrète pour discussions

#### 3. Tests d'acceptation utilisateur (UAT)

**Processus:**
```
1. Préparation
   ├─ Rédaction des scénarios de test
   ├─ Préparation des données de test
   ├─ Sélection des testeurs (utilisateurs finaux)
   └─ Formation des testeurs

2. Exécution (1-2 semaines)
   ├─ Tests en conditions réelles
   ├─ Suivi par formulaire de feedback
   ├─ Sessions d'observation
   └─ Collecte des anomalies

3. Analyse
   ├─ Consolidation des retours
   ├─ Priorisation des corrections
   ├─ Décision Go/No-go
   └─ Plan d'actions

4. Clôture
   ├─ Corrections implémentées
   ├─ Re-tests si nécessaire
   └─ Validation finale par sponsor
```

**Formulaire de test UAT:**
```
Scénario: Emprunter un livre

Étapes:
1. Se connecter avec identifiants fournis      [✓] [✗] [NA]
   Résultat: ________________________________
   
2. Rechercher "Clean Code"                     [✓] [✗] [NA]
   Résultat: ________________________________
   
3. Cliquer sur "Emprunter"                     [✓] [✗] [NA]
   Résultat: ________________________________
   
4. Vérifier confirmation email reçu            [✓] [✗] [NA]
   Résultat: ________________________________

Satisfaction générale:  😀  🙂  😐  🙁  😞

Commentaires libres:
_________________________________________________
_________________________________________________

Problèmes rencontrés:
_________________________________________________
_________________________________________________
```

#### 4. Matrice de traçabilité

**Traçabilité complète: Besoin → Exigence → Test → Code**

| ID Besoin | Exigence | Use Case | Classe | Tests | Statut |
|-----------|----------|----------|--------|-------|---------|
| BES-001 | EX-FUNC-001 | UC-001 | AdherentService.emprunter() | UT-001, IT-003, UAT-001 | ✅ |
| BES-001 | EX-FUNC-002 | UC-001 | EmpruntRepository.save() | UT-005, IT-003 | ✅ |
| BES-002 | EX-FUNC-010 | UC-005 | RechercheService.search() | UT-020, IT-010, UAT-005 | 🔄 |
| BES-003 | EX-PERF-001 | - | CacheManager | PT-001 | ⏳ |

**Avantages:**
- Garantit que chaque besoin est implémenté et testé
- Facilite l'analyse d'impact lors de changements
- Démontre la couverture complète

### 2.4.2 Outils de gestion des exigences

| Outil | Type | Points forts | Idéal pour |
|-------|------|--------------|------------|
| **Jira + Confluence** | Agile | Intégration dev/ops, extensible | Équipes Scrum/Kanban |
| **Azure DevOps** | Enterprise | Suite complète Microsoft | Environnements .NET |
| **IBM DOORS** | Requirements Mgmt | Traçabilité avancée, normes | Projets critiques, réglementés |
| **Polarion** | ALM | Gestion cycle complet | Automotive, aérospatial |
| **ReqSuite** | Requirements | Spécialisé exigences | Analyse métier |
| **Notion/Confluence** | Documentation | Collaboration, flexibilité | Startups, petites équipes |

---

## 📝 Résumé du chapitre

### Points clés

✅ Les **user stories** suivent le format As-a/I-want/So-that avec critères d'acceptation

✅ **UML** fournit des diagrammes standardisés pour modéliser le système
   - Cas d'utilisation: vue fonctionnelle
   - Séquence: interactions dynamiques
   - Classes: structure statique

✅ Les **méthodes d'analyse** (MERISE, CRUD, Persona) aident à structurer l'analyse

✅ La **validation des exigences** passe par revues, prototypage, tests UAT

✅ La **traçabilité** garantit l'alignement besoin-code-test

---

## 💡 Exercices pratiques

### Exercice 1: Rédiger des user stories

Pour un système de réservation de salles de réunion, rédigez 3 user stories avec critères d'acceptation.

### Exercice 2: Diagramme de cas d'utilisation

Créez un diagramme de cas d'utilisation pour une application de livraison de repas avec 3 acteurs: Client, Restaurant, Livreur.

### Exercice 3: Diagramme de séquence

Modélisez en diagramme de séquence le processus: "Un client passe une commande et reçoit une confirmation par email".

---

## 📚 Pour aller plus loin

- 📖 **"UML 2 par la pratique"** - Pascal Roques
- 📖 **"Analyzing Business Data with Excel"** - Gerald Knight
- 🔗 **PlantUML**: Outil pour générer des diagrammes UML à partir de texte
- 🔗 **Draw.io / Lucidchart**: Outils de modélisation visuels

---

[⬅️ Chapitre 1](./chapitre-1-cycle-vie-logiciel.md) | [➡️ Chapitre 3](./chapitre-3-architecture-logicielle.md)

