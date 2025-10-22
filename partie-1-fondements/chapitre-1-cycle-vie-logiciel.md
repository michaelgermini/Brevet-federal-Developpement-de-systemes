# Chapitre 1 : Comprendre le cycle de vie logiciel

> **Objectif:** Maîtriser les différents modèles de cycle de vie logiciel, comprendre la gestion des exigences et établir le lien entre analyse métier et conception technique.

---

## 📋 Table des matières

1. [Définitions et terminologies clés](#11-définitions-et-terminologies-clés)
2. [Cycle en V, modèle en spirale, et méthodes itératives](#12-cycle-en-v-modèle-en-spirale-et-méthodes-itératives)
3. [Gestion des exigences et cahier des charges](#13-gestion-des-exigences-et-cahier-des-charges)
4. [Lien entre analyse métier et conception technique](#14-lien-entre-analyse-métier-et-conception-technique)

---

## 1.1 Définitions et terminologies clés

### 1.1.1 Qu'est-ce qu'un cycle de vie logiciel?

Le **cycle de vie logiciel** (Software Development Life Cycle - SDLC) représente l'ensemble des phases par lesquelles passe un système logiciel depuis sa conception jusqu'à sa mise hors service. Il structure et organise le processus de développement pour garantir la qualité, la traçabilité et la maintenabilité du produit final.

**Définition formelle:**
> Le SDLC est un processus structuré de planification, création, test, déploiement et maintenance d'un système d'information, visant à produire un logiciel de haute qualité dans des délais et budgets maîtrisés.

### 1.1.2 Les phases universelles d'un projet logiciel

Quel que soit le modèle choisi, tout projet logiciel traverse ces phases essentielles:

#### 📊 Phase 1: Analyse et planification
- **Étude de faisabilité:** Évaluation technique, économique et opérationnelle
- **Analyse des besoins:** Collecte et formalisation des attentes
- **Planification:** Estimation des ressources, délais et coûts

**Livrables typiques:**
- Document de faisabilité
- Étude d'opportunité
- Planning prévisionnel
- Budget prévisionnel

#### 🎨 Phase 2: Conception (Design)
- **Conception architecturale:** Structure globale du système
- **Conception détaillée:** Spécifications techniques précises
- **Modélisation:** Diagrammes UML, schémas de bases de données

**Livrables typiques:**
- Document d'architecture technique
- Spécifications fonctionnelles détaillées
- Modèles de données
- Diagrammes UML

#### 💻 Phase 3: Développement (Implémentation)
- **Codage:** Écriture du code source
- **Intégration:** Assemblage des composants
- **Revue de code:** Contrôle qualité du code

**Livrables typiques:**
- Code source
- Documentation du code
- Scripts de déploiement
- Bibliothèques et dépendances

#### ✅ Phase 4: Tests et validation
- **Tests unitaires:** Validation des composants individuels
- **Tests d'intégration:** Validation des interactions
- **Tests système:** Validation globale
- **Tests d'acceptation:** Validation par les utilisateurs

**Livrables typiques:**
- Plans de tests
- Rapports de tests
- Listes de bugs et résolutions
- Certificat de recette

#### 🚀 Phase 5: Déploiement et mise en production
- **Préparation de l'environnement:** Configuration serveurs, bases de données
- **Migration des données:** Transfert depuis l'ancien système
- **Formation des utilisateurs:** Documentation et sessions de formation
- **Go-live:** Bascule en production

**Livrables typiques:**
- Procédures de déploiement
- Documentation utilisateur
- Guides d'administration
- Plan de formation

#### 🔧 Phase 6: Maintenance et évolution
- **Maintenance corrective:** Correction des bugs
- **Maintenance adaptative:** Adaptations à l'environnement
- **Maintenance évolutive:** Nouvelles fonctionnalités
- **Maintenance préventive:** Optimisations et améliorations

**Livrables typiques:**
- Patches et mises à jour
- Notes de version (release notes)
- Rapports d'incidents
- Plans d'évolution

### 1.1.3 Terminologies essentielles

| Terme | Définition | Exemple |
|-------|------------|---------|
| **Exigence fonctionnelle** | Fonction que le système doit réaliser | "L'utilisateur doit pouvoir réinitialiser son mot de passe" |
| **Exigence non fonctionnelle** | Contrainte ou qualité du système | "Le temps de réponse doit être < 2 secondes" |
| **User Story** | Description courte d'une fonctionnalité du point de vue utilisateur | "En tant que client, je veux pouvoir filtrer les produits par prix" |
| **Backlog** | Liste priorisée des fonctionnalités à développer | Ensemble des user stories en attente |
| **Sprint** | Période de temps fixe pour développer un ensemble de fonctionnalités | Sprint de 2 semaines |
| **Livrable** | Produit ou document résultant d'une phase | Code source, documentation, rapport de tests |
| **Jalons (Milestones)** | Points de contrôle majeurs dans le projet | Fin de la conception, mise en production |
| **Stakeholder** | Partie prenante ayant un intérêt dans le projet | Client, utilisateurs, direction, équipe |

### 1.1.4 Métriques et indicateurs

Pour piloter efficacement un projet, plusieurs indicateurs sont essentiels:

**Indicateurs de progression:**
- **Avancement physique:** Pourcentage de tâches terminées
- **Vélocité:** Quantité de travail réalisée par itération (en Agile)
- **Burn-down chart:** Visualisation du travail restant

**Indicateurs de qualité:**
- **Densité de défauts:** Nombre de bugs par 1000 lignes de code
- **Couverture de tests:** Pourcentage de code couvert par les tests
- **Dette technique:** Effort estimé pour corriger les imperfections

**Indicateurs de coût:**
- **Budget consommé:** Dépenses réelles vs prévues
- **Coût par fonctionnalité:** Budget / nombre de fonctionnalités
- **ROI (Return on Investment):** Bénéfices / Coûts

---

## 1.2 Cycle en V, modèle en spirale, et méthodes itératives

### 1.2.1 Le modèle en cascade (Waterfall)

**Principe:** Approche séquentielle où chaque phase doit être complétée avant de passer à la suivante.

```
┌─────────────────┐
│   Analyse       │
└────────┬────────┘
         │
┌────────▼────────┐
│  Conception     │
└────────┬────────┘
         │
┌────────▼────────┐
│ Développement   │
└────────┬────────┘
         │
┌────────▼────────┐
│     Tests       │
└────────┬────────┘
         │
┌────────▼────────┐
│  Déploiement    │
└────────┬────────┘
         │
┌────────▼────────┐
│   Maintenance   │
└─────────────────┘
```

**✅ Avantages:**
- Simple à comprendre et à gérer
- Documentation complète et structurée
- Adapté aux projets avec exigences stables et bien définies
- Facilite la planification et l'estimation

**❌ Inconvénients:**
- Rigide et peu flexible face aux changements
- Retour utilisateur tardif (en fin de projet)
- Risques découverts tard dans le processus
- Pas adapté aux projets innovants ou exploratoires

**🎯 Cas d'usage typiques:**
- Projets réglementaires avec spécifications figées
- Systèmes critiques (aéronautique, médical)
- Projets avec budget et délai fixes
- Migration de systèmes existants bien documentés

### 1.2.2 Le cycle en V

**Principe:** Extension du modèle en cascade intégrant explicitement les phases de validation et vérification.

```
        Conception                    Validation
              
    Spécifications  ←──────────→  Tests d'acceptation
         │                              ▲
         │                              │
    Architecture    ←──────────→  Tests d'intégration
         │                              ▲
         │                              │
 Conception détaillée ←────────→   Tests unitaires
         │                              ▲
         │                              │
         └──────→  Codage  ──────────────┘
```

**Caractéristiques clés:**
- **Correspondance:** Chaque phase de conception a une phase de test correspondante
- **Traçabilité:** Lien direct entre exigences et tests de validation
- **Documentation:** Forte emphase sur la documentation à chaque niveau

**Phases descendantes (gauche):**
1. **Analyse des besoins** → définition des tests d'acceptation
2. **Spécifications fonctionnelles** → définition des tests fonctionnels
3. **Conception architecturale** → définition des tests d'intégration
4. **Conception détaillée** → définition des tests unitaires
5. **Codage** (bas du V)

**Phases ascendantes (droite):**
1. **Tests unitaires** ← vérification de la conception détaillée
2. **Tests d'intégration** ← vérification de l'architecture
3. **Tests fonctionnels** ← validation des spécifications
4. **Tests d'acceptation** ← validation des besoins

**✅ Avantages:**
- Approche rigoureuse et systématique
- Forte traçabilité entre exigences et tests
- Détection précoce des erreurs de conception
- Qualité élevée du produit final

**❌ Inconvénients:**
- Peu flexible face aux changements
- Coût élevé des modifications tardives
- Nécessite des spécifications très précises dès le début
- Délai avant la première livraison

**🎯 Cas d'usage typiques:**
- Systèmes embarqués
- Applications critiques (banque, santé, aéronautique)
- Projets à forte exigence de traçabilité
- Développement de systèmes avec certification

**Exemple concret: Système de gestion bancaire**

```
Phase 1: Analyse des besoins
└─ Exigence: "Le système doit permettre les virements sécurisés"
   └─ Test d'acceptation: Scénarios de virements avec vérifications

Phase 2: Spécifications fonctionnelles
└─ Fonction: Module de transaction avec authentification forte
   └─ Test fonctionnel: Validation du workflow complet

Phase 3: Architecture
└─ Composant: Service de transaction + Base de données
   └─ Test d'intégration: Communication entre services

Phase 4: Conception détaillée
└─ Classe: TransactionService avec méthode processTransfer()
   └─ Test unitaire: Test de la méthode processTransfer()
```

### 1.2.3 Le modèle en spirale

**Principe:** Approche itérative combinant développement et gestion des risques, créé par Barry Boehm en 1988.

```
                  Déterminer
                  objectifs
                      │
                      ▼
    Planifier    ◄───────►    Évaluer
    prochaine                 risques
    itération                     │
         ▲                        │
         │                        ▼
         └────────────────   Développer
                          et tester
```

**Les 4 quadrants de chaque itération:**

1. **Définition des objectifs (Quadrant 1)**
   - Identifier les objectifs de l'itération
   - Définir les alternatives possibles
   - Identifier les contraintes

2. **Analyse des risques (Quadrant 2)**
   - Évaluer les alternatives
   - Identifier et résoudre les risques
   - Prototypage si nécessaire

3. **Développement et test (Quadrant 3)**
   - Développer le produit
   - Valider par des tests
   - Intégrer les retours

4. **Planification (Quadrant 4)**
   - Évaluer les résultats
   - Planifier la prochaine itération
   - Décision de continuer ou arrêter

**✅ Avantages:**
- Gestion proactive des risques
- Flexibilité et adaptabilité
- Validation continue avec les utilisateurs
- Détection précoce des problèmes

**❌ Inconvénients:**
- Complexité de gestion
- Nécessite une expertise en gestion des risques
- Peut être coûteux
- Difficulté d'estimation des coûts totaux

**🎯 Cas d'usage typiques:**
- Projets innovants avec risques techniques élevés
- Grands systèmes complexes
- Projets de R&D
- Développement de nouveaux produits

**Exemple d'itération:**

```
Itération 1: Preuve de concept
├─ Objectif: Valider la faisabilité technique
├─ Risques: Compatibilité avec système existant
├─ Développement: Prototype minimal
└─ Résultat: Go/No-go pour la suite

Itération 2: Architecture de base
├─ Objectif: Implémenter l'architecture principale
├─ Risques: Performance insuffisante
├─ Développement: Framework de base + tests de charge
└─ Résultat: Architecture validée

Itération 3: Fonctionnalités principales
├─ Objectif: Développer les 3 fonctions critiques
├─ Risques: Complexité d'intégration
├─ Développement: Modules + tests d'intégration
└─ Résultat: MVP fonctionnel
```

### 1.2.4 Les méthodes itératives et incrémentales

**Principe:** Développement par cycles courts produisant des versions successives de plus en plus complètes.

**Différence entre itératif et incrémental:**

```
Incrémental: Construction progressive
┌─────┐  ┌─────┐  ┌─────┐  ┌─────┐
│  A  │  │ A+B │  │A+B+C│  │ABCD │
└─────┘  └─────┘  └─────┘  └─────┘
 V1.0     V1.1     V1.2     V1.3

Itératif: Raffinement successif
┌─────┐  ┌─────┐  ┌─────┐  ┌─────┐
│ A₁  │  │ A₂  │  │ A₃  │  │ A₄  │
└─────┘  └─────┘  └─────┘  └─────┘
Version  Version  Version  Version
basique  améliorée optimale  finale
```

**Caractéristiques:**
- **Cycles courts:** 2-4 semaines généralement
- **Livrables fonctionnels:** Chaque itération produit du code exécutable
- **Feedback continu:** Validation régulière avec les utilisateurs
- **Adaptation:** Ajustement du plan selon les retours

**Processus typique d'une itération:**

```
1. Planification de l'itération (1 jour)
   └─ Sélection des user stories
   └─ Découpage en tâches
   └─ Estimation

2. Développement (8-10 jours)
   └─ Conception
   └─ Codage
   └─ Tests unitaires
   └─ Revues de code

3. Intégration et tests (2-3 jours)
   └─ Tests d'intégration
   └─ Tests fonctionnels
   └─ Correction de bugs

4. Démonstration et rétrospective (1 jour)
   └─ Présentation aux stakeholders
   └─ Collecte de feedback
   └─ Amélioration du processus
```

**✅ Avantages:**
- Retour rapide des utilisateurs
- Réduction des risques
- Meilleure gestion du changement
- Visibilité continue sur l'avancement
- Motivation de l'équipe

**❌ Inconvénients:**
- Nécessite une forte implication des utilisateurs
- Peut manquer de vision d'ensemble
- Risque d'architecture émergente non optimale
- Nécessite une équipe mature

### 1.2.5 Comparaison des modèles

| Critère | Cascade | Cycle en V | Spirale | Itératif |
|---------|---------|------------|---------|----------|
| **Flexibilité** | ⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Gestion des risques** | ⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Documentation** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| **Feedback utilisateur** | ⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Prévisibilité** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐ |
| **Complexité de gestion** | ⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Adapté aux changements** | ⭐ | ⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

**Critères de choix:**

```
Choisir Cascade/V si:
✓ Exigences claires et stables
✓ Projet bien défini et documenté
✓ Domaine réglementé
✓ Faible tolérance aux changements

Choisir Spirale si:
✓ Projet innovant et risqué
✓ Budget flexible
✓ Expertise en gestion des risques
✓ Projet de grande envergure

Choisir Itératif si:
✓ Exigences évolutives
✓ Feedback utilisateur important
✓ Time-to-market critique
✓ Équipe agile et autonome
```

---

## 1.3 Gestion des exigences et cahier des charges

### 1.3.1 Qu'est-ce qu'une exigence?

**Définition:**
> Une exigence est une caractéristique, fonction ou contrainte qu'un système doit satisfaire pour répondre aux besoins de ses parties prenantes.

**Propriétés d'une bonne exigence (SMART):**
- **Spécifique:** Décrit précisément ce qui est attendu
- **Mesurable:** Permet la vérification objective
- **Atteignable:** Réalisable techniquement et économiquement
- **Réaliste:** Cohérent avec les contraintes du projet
- **Temporel:** Situé dans le temps (délai, priorité)

### 1.3.2 Types d'exigences

#### Exigences fonctionnelles (Functional Requirements)

Décrivent **ce que le système doit faire** - les fonctionnalités et services.

**Exemples:**
```
EX-FUNC-001: L'utilisateur doit pouvoir créer un compte avec email et mot de passe
EX-FUNC-002: Le système doit envoyer un email de confirmation après l'inscription
EX-FUNC-003: L'utilisateur doit pouvoir rechercher des produits par catégorie
EX-FUNC-004: Le système doit calculer automatiquement le prix total avec TVA
```

**Structure recommandée:**
```
ID: [Identifiant unique]
Titre: [Nom court de l'exigence]
Description: [Description détaillée]
Priorité: [Critique/Haute/Moyenne/Basse]
Acteur: [Qui utilise cette fonction]
Préconditions: [Conditions préalables]
Postconditions: [Résultat attendu]
Cas d'exception: [Situations d'erreur]
```

#### Exigences non fonctionnelles (Non-Functional Requirements)

Décrivent **comment le système doit fonctionner** - les qualités et contraintes.

**Catégories principales:**

**1. Performance**
```
EX-PERF-001: Le temps de réponse des API doit être < 200ms pour 95% des requêtes
EX-PERF-002: Le système doit supporter 10 000 utilisateurs simultanés
EX-PERF-003: Le démarrage de l'application doit prendre moins de 3 secondes
```

**2. Sécurité**
```
EX-SEC-001: Les mots de passe doivent être hashés avec bcrypt (min. 12 rounds)
EX-SEC-002: Les communications doivent utiliser TLS 1.3 minimum
EX-SEC-003: Le système doit implémenter l'authentification à deux facteurs
```

**3. Disponibilité et fiabilité**
```
EX-DISP-001: Le système doit avoir une disponibilité de 99.9% (SLA)
EX-DISP-002: Le temps de récupération après panne doit être < 15 minutes
EX-FIAB-001: Le taux d'échec des transactions doit être < 0.1%
```

**4. Utilisabilité**
```
EX-UTIL-001: L'interface doit être responsive (mobile, tablet, desktop)
EX-UTIL-002: Le système doit supporter les lecteurs d'écran (WCAG 2.1 niveau AA)
EX-UTIL-003: La navigation principale doit être accessible en < 3 clics
```

**5. Maintenabilité**
```
EX-MAINT-001: Le code doit avoir une couverture de tests > 80%
EX-MAINT-002: La documentation technique doit être à jour
EX-MAINT-003: Le code doit respecter les standards ESLint/SonarQube
```

**6. Compatibilité**
```
EX-COMP-001: L'application doit fonctionner sur Chrome, Firefox, Safari, Edge
EX-COMP-002: L'API doit être compatible avec les versions mobiles iOS 14+ et Android 10+
EX-COMP-003: Le système doit s'intégrer avec Active Directory
```

**7. Scalabilité**
```
EX-SCAL-001: L'architecture doit permettre un scaling horizontal
EX-SCAL-002: La base de données doit supporter un partitionnement (sharding)
```

**8. Légales et réglementaires**
```
EX-LEG-001: Le système doit être conforme au RGPD
EX-LEG-002: Les logs d'audit doivent être conservés 7 ans
EX-LEG-003: Le consentement utilisateur doit être explicite et révocable
```

### 1.3.3 Collecte des exigences

**Techniques de collecte:**

#### 1. Entretiens (Interviews)
**Approche:** Discussion en tête-à-tête avec les parties prenantes

```
Préparation:
├─ Identifier les stakeholders clés
├─ Préparer les questions
├─ Définir les objectifs de l'entretien
└─ Prévoir 60-90 minutes

Conduite:
├─ Questions ouvertes: "Comment gérez-vous actuellement...?"
├─ Questions fermées: "Avez-vous besoin de...?"
├─ Écoute active et prise de notes
└─ Demande d'exemples concrets

Synthèse:
├─ Rédaction du compte-rendu
├─ Validation avec l'interviewé
└─ Extraction des exigences
```

**✅ Avantages:** Informations détaillées, clarification immédiate  
**❌ Inconvénients:** Chronophage, dépend de la qualité de communication

#### 2. Ateliers (Workshops)
**Approche:** Sessions collaboratives avec plusieurs stakeholders

```
Organisation:
├─ 5-10 participants
├─ Durée: 2-4 heures
├─ Facilitateur + scribe
└─ Salle avec tableau blanc/paperboard

Techniques:
├─ Brainstorming
├─ Cartes mentales (mind mapping)
├─ Priorisation (MoSCoW, Dot Voting)
└─ Story mapping

Résultat:
├─ Vision partagée
├─ Liste d'exigences priorisées
└─ Consensus sur les priorités
```

**✅ Avantages:** Consensus rapide, créativité collective  
**❌ Inconvénients:** Peut être dominé par certaines voix, nécessite bonne animation

#### 3. Questionnaires
**Approche:** Collecte structurée d'informations auprès d'un large public

```
Conception:
├─ Questions claires et concises
├─ Mix de questions fermées et ouvertes
├─ 10-20 questions maximum
└─ Temps estimé: 10-15 minutes

Distribution:
├─ Email, formulaire en ligne (Google Forms, Typeform)
├─ Population cible clairement identifiée
└─ Relances si nécessaire

Analyse:
├─ Statistiques descriptives
├─ Identification des tendances
└─ Extraction des exigences récurrentes
```

**✅ Avantages:** Nombreux retours, quantifiable  
**❌ Inconvénients:** Pas de nuances, taux de réponse variable

#### 4. Observation
**Approche:** Observer les utilisateurs dans leur environnement de travail

```
Préparation:
├─ Définir les objectifs d'observation
├─ Planifier les sessions
├─ Informer et obtenir le consentement
└─ Préparer une grille d'observation

Observation:
├─ Noter les actions effectuées
├─ Identifier les difficultés
├─ Repérer les workarounds
├─ Mesurer les temps
└─ Poser des questions de clarification

Analyse:
├─ Identifier les processus inefficaces
├─ Détecter les besoins non exprimés
└─ Formuler des exigences d'amélioration
```

**✅ Avantages:** Détecte les besoins implicites, objectif  
**❌ Inconvénients:** Effet Hawthorne (comportement modifié par observation), chronophage

#### 5. Analyse de documents
**Approche:** Étudier la documentation existante

```
Sources:
├─ Procédures opérationnelles
├─ Manuels utilisateurs actuels
├─ Rapports et statistiques
├─ Réglementation et normes
├─ Contrats et SLA
└─ Documentation technique existante

Analyse:
├─ Identifier les fonctionnalités actuelles
├─ Repérer les contraintes légales
├─ Comprendre les processus métier
└─ Extraire les exigences de compatibilité
```

**✅ Avantages:** Non intrusif, contextuel  
**❌ Inconvénients:** Documentation parfois obsolète ou incomplète

#### 6. Prototypage
**Approche:** Créer une maquette pour faire réagir les utilisateurs

```
Types de prototypes:
├─ Wireframes: Structure basique
├─ Mockups: Design visuel
├─ Prototypes interactifs: Simulation des interactions
└─ POC (Proof of Concept): Validation technique

Processus:
1. Créer le prototype (basse/haute fidélité)
2. Présenter aux utilisateurs
3. Recueillir les retours
4. Itérer sur le prototype
5. Formaliser les exigences validées
```

**✅ Avantages:** Concret, retour visuel immédiat  
**❌ Inconvénients:** Risque de focus sur l'UI plutôt que les fonctionnalités

### 1.3.4 Le cahier des charges (Spécification des Exigences Logicielles - SRS)

**Définition:**
> Le cahier des charges (ou SRS - Software Requirements Specification) est le document formel qui décrit de manière complète et non ambiguë ce que le système doit faire.

**Structure type d'un SRS selon IEEE 830:**

```
1. INTRODUCTION
   1.1 Objectif du document
   1.2 Portée du système
   1.3 Définitions, acronymes et abréviations
   1.4 Références
   1.5 Vue d'ensemble du document

2. DESCRIPTION GÉNÉRALE
   2.1 Perspective du produit
       ├─ Interfaces systèmes
       ├─ Interfaces utilisateurs
       ├─ Interfaces matérielles
       └─ Interfaces logicielles
   2.2 Fonctions du produit (vue d'ensemble)
   2.3 Caractéristiques des utilisateurs
   2.4 Contraintes générales
   2.5 Hypothèses et dépendances

3. EXIGENCES SPÉCIFIQUES
   3.1 Exigences fonctionnelles
       ├─ Cas d'utilisation
       ├─ Diagrammes de séquence
       └─ Description détaillée de chaque fonction
   3.2 Exigences non fonctionnelles
       ├─ Performance
       ├─ Sécurité
       ├─ Fiabilité
       ├─ Disponibilité
       └─ Maintenabilité
   3.3 Exigences d'interface
       ├─ Interface utilisateur
       ├─ Interface logicielle (API)
       └─ Interface matérielle
   3.4 Exigences de données
       ├─ Modèle de données
       └─ Règles de gestion

4. MODÈLES UML
   4.1 Diagrammes de cas d'utilisation
   4.2 Diagrammes de classes (conceptuel)
   4.3 Diagrammes d'activité
   4.4 Diagrammes d'états

5. ANNEXES
   5.1 Glossaire
   5.2 Modèles de documents
   5.3 Références techniques
```

**Exemple concret - Extrait d'un SRS pour un système e-commerce:**

```markdown
3.1.2 Gestion du panier d'achat

EX-FUNC-012: Ajout d'un produit au panier
├─ Priorité: Critique
├─ Acteur: Client (authentifié ou anonyme)
├─ Préconditions: 
│   └─ Le produit existe et est disponible en stock
├─ Description:
│   Le client peut ajouter un produit à son panier en spécifiant la quantité.
│   Le système vérifie la disponibilité et met à jour le panier.
├─ Scénario nominal:
│   1. Le client visualise la fiche produit
│   2. Le client sélectionne la quantité souhaitée
│   3. Le client clique sur "Ajouter au panier"
│   4. Le système vérifie la disponibilité du stock
│   5. Le système ajoute le produit au panier
│   6. Le système affiche une confirmation
│   7. Le système met à jour le compteur du panier
├─ Scénarios alternatifs:
│   A1. Stock insuffisant
│       4a. Le système affiche la quantité maximum disponible
│       4b. Le système propose d'ajuster la quantité
│   A2. Client non authentifié
│       5a. Le panier est stocké en session locale
│       5b. À la connexion, fusion avec le panier serveur
├─ Postconditions:
│   └─ Le produit est dans le panier avec la quantité spécifiée
├─ Règles métier:
│   ├─ RG-001: Maximum 10 unités par produit
│   ├─ RG-002: Vérification du stock en temps réel
│   └─ RG-003: Panier anonyme conservé 24h
└─ Critères d'acceptation:
    ├─ Le produit apparaît dans le panier
    ├─ La quantité est correcte
    ├─ Le prix total est mis à jour
    └─ Le message de confirmation s'affiche
```

**Bonnes pratiques de rédaction:**

✅ **Utiliser un langage clair et précis**
```
❌ "Le système doit être rapide"
✅ "Le système doit afficher la page d'accueil en moins de 2 secondes"
```

✅ **Une exigence = une fonctionnalité**
```
❌ "Le système doit permettre la connexion et l'inscription"
✅ "EX-001: Le système doit permettre la connexion"
   "EX-002: Le système doit permettre l'inscription"
```

✅ **Éviter les termes ambigus**
```
❌ "adapté", "flexible", "user-friendly", "robuste"
✅ "conforme à WCAG 2.1", "extensible via plugins", "disponible à 99.9%"
```

✅ **Traçabilité**
```
Chaque exigence doit avoir:
├─ Identifiant unique
├─ Origine (qui l'a demandée)
├─ Justification (pourquoi)
└─ Critères de validation (comment tester)
```

### 1.3.5 Gestion et traçabilité des exigences

**Matrice de traçabilité:**

| ID Exigence | Origine | Cas d'utilisation | Composant | Tests | Statut |
|-------------|---------|-------------------|-----------|-------|---------|
| EX-FUNC-001 | Client | UC-001 | UserService | UT-001, IT-005 | ✅ Validé |
| EX-FUNC-002 | RGPD | UC-002, UC-003 | DataService | UT-010, IT-008 | 🔄 En cours |
| EX-PERF-001 | Direction | - | Cache Layer | PT-001 | ⏳ Planifié |

**Outils de gestion des exigences:**
- **Jira + Confluence:** Gestion agile avec traçabilité
- **Azure DevOps:** Intégration complète (exigences → code → tests)
- **IBM DOORS:** Gestion avancée pour projets critiques
- **Caliber:** Traçabilité et gestion des changements
- **Tableau Excel:** Solution simple pour petits projets

**Gestion des changements d'exigences:**

```
Processus de change management:

1. Demande de changement
   ├─ Formulaire standardisé
   ├─ Justification
   └─ Impact estimé

2. Analyse d'impact
   ├─ Impact fonctionnel
   ├─ Impact technique
   ├─ Impact planning/coût
   └─ Impact qualité

3. Décision
   ├─ Comité de changement
   ├─ Approbation ou rejet
   └─ Priorisation

4. Implémentation
   ├─ Mise à jour de la documentation
   ├─ Communication aux équipes
   ├─ Développement
   └─ Validation

5. Traçabilité
   ├─ Historique des modifications
   ├─ Versions du SRS
   └─ Justifications
```

---

## 1.4 Lien entre analyse métier et conception technique

### 1.4.1 De la vision métier à la réalité technique

**Le pont entre deux mondes:**

```
Monde Métier                    Monde Technique
(Langage business)             (Langage IT)

┌──────────────┐              ┌──────────────┐
│   Besoins    │              │ Architecture │
│   métier     │◄────────────►│  technique   │
└──────────────┘              └──────────────┘
       │                             │
       │ Traduction                  │
       ▼                             ▼
┌──────────────┐              ┌──────────────┐
│  Processus   │              │  Composants  │
│   métier     │◄────────────►│  logiciels   │
└──────────────┘              └──────────────┘
       │                             │
       │ Formalisation               │
       ▼                             ▼
┌──────────────┐              ┌──────────────┐
│ Règles de    │              │  Code source │
│  gestion     │◄────────────►│  et données  │
└──────────────┘              └──────────────┘
```

**Rôle du développeur de systèmes:**
- 🎧 **Écouter** et comprendre le langage métier
- 🔄 **Traduire** les besoins en spécifications techniques
- 🏗️ **Concevoir** une architecture adaptée
- ⚖️ **Arbitrer** entre contraintes métier et faisabilité technique
- 🔗 **Assurer** la cohérence entre vision métier et implémentation

### 1.4.2 Techniques de modélisation orientée métier

#### Domain-Driven Design (DDD)

**Principe:** Placer le domaine métier au cœur de la conception logicielle.

**Concepts clés:**

**1. Ubiquitous Language (Langage omniprésent)**
```
Créer un vocabulaire commun entre métier et technique:

Métier dit:              Code contient:
"Client"          →      class Customer
"Commande"        →      class Order
"Valider"         →      method validate()
"En attente"      →      enum Status.PENDING
```

**2. Bounded Context (Contexte délimité)**
```
Diviser le système en contextes avec leur propre modèle:

Contexte Ventes:
├─ Client (avec historique d'achats)
├─ Commande
└─ Catalogue produits

Contexte Livraison:
├─ Destinataire (adresse de livraison)
├─ Colis
└─ Statut de livraison

Le même concept ("Client") peut avoir des significations différentes
dans chaque contexte !
```

**3. Entities, Value Objects, Aggregates**
```java
// Entity: Objet avec identité unique
public class Order {
    private OrderId id;  // Identité
    private Customer customer;
    private List<OrderLine> lines;
    private OrderStatus status;
    
    // Logique métier encapsulée
    public void addLine(Product product, int quantity) {
        if (status != OrderStatus.DRAFT) {
            throw new OrderClosedException();
        }
        lines.add(new OrderLine(product, quantity));
    }
}

// Value Object: Objet sans identité, défini par ses attributs
public class Money {
    private final BigDecimal amount;
    private final Currency currency;
    
    // Immutable
    public Money add(Money other) {
        if (!this.currency.equals(other.currency)) {
            throw new CurrencyMismatchException();
        }
        return new Money(this.amount.add(other.amount), this.currency);
    }
}

// Aggregate: Groupe d'objets traités comme une unité
public class Order {  // Aggregate Root
    private List<OrderLine> lines;  // Part of the aggregate
    
    // Invariants métier garantis
    public void removeAllLinesForProduct(ProductId productId) {
        lines.removeIf(line -> line.getProductId().equals(productId));
        recalculateTotal();  // Maintien de la cohérence
    }
}
```

**4. Services de domaine**
```java
// Logique métier qui ne relève pas d'une entité spécifique
public class PricingService {
    public Money calculateOrderTotal(Order order, Customer customer) {
        Money subtotal = order.getSubtotal();
        Money discount = calculateDiscount(customer, subtotal);
        Money tax = calculateTax(subtotal.subtract(discount));
        return subtotal.subtract(discount).add(tax);
    }
    
    private Money calculateDiscount(Customer customer, Money amount) {
        // Règles métier complexes de remise
        if (customer.isVIP()) {
            return amount.multiply(0.10);
        } else if (customer.hasLoyaltyCard()) {
            return amount.multiply(0.05);
        }
        return Money.zero(amount.getCurrency());
    }
}
```

#### Event Storming

**Principe:** Atelier collaboratif pour modéliser les processus métier via les événements.

**Processus:**
```
1. Identifier les Domain Events (événements métier)
   Orange: "CommandePassée", "PaiementReçu", "ComandeExpédiée"

2. Ordonner chronologiquement
   Timeline: événements dans l'ordre temporel

3. Identifier les Commands (actions)
   Bleu: "PasserCommande", "PayerCommande", "ExpédierCommande"

4. Identifier les Aggregates
   Jaune: "Commande", "Paiement", "Expédition"

5. Identifier les Policies (règles)
   Violet: "Quand PaiementReçu, alors notifier entrepôt"

6. Identifier les Read Models (vues)
   Vert: "Liste des commandes", "Tableau de bord"
```

**Exemple visuel:**
```
[PasserCommande] → [CommandePassée] → [Vérifier Stock]
                         ↓
                   [Commande] (Aggregate)
                         ↓
              [Stock insuffisant] ?
                   ↙        ↘
               [OUI]        [NON]
                 ↓            ↓
    [CommandeAnnulée]  [CommandeConfirmée]
                              ↓
                       [Traiter Paiement]
                              ↓
                       [PaiementReçu]
                              ↓
                    Policy: Notifier entrepôt
                              ↓
                       [ExpédierCommande]
```

**✅ Avantages:**
- Collaboration métier/IT
- Compréhension partagée
- Identification des processus complexes
- Base pour architecture événementielle

### 1.4.3 Du modèle métier au code

**Exemple complet: Système de gestion de bibliothèque**

#### Étape 1: Analyse métier

**Processus métier identifié:**
```
Un adhérent peut emprunter jusqu'à 5 livres simultanément.
La durée d'emprunt est de 21 jours.
Si le livre n'est pas rendu à temps, des frais de retard sont appliqués.
Un adhérent avec des frais impayés ne peut pas emprunter.
```

#### Étape 2: Modélisation (UML Domaine)

```
┌─────────────────┐         ┌─────────────────┐
│    Adhérent     │         │      Livre      │
├─────────────────┤         ├─────────────────┤
│ - id            │         │ - id            │
│ - nom           │         │ - ISBN          │
│ - email         │         │ - titre         │
│ - fraisImpayés  │◄───────►│ - auteur        │
├─────────────────┤    0..* ├─────────────────┤
│ + emprunter()   │    1..* │ + estDisponible()│
│ + rendre()      │         │                 │
│ + peutEmprunter()│        │                 │
└─────────────────┘         └─────────────────┘
         △                           △
         │                           │
         │    ┌─────────────────┐   │
         └────│    Emprunt      │───┘
              ├─────────────────┤
              │ - id            │
              │ - dateEmprunt   │
              │ - dateRetourPrévue│
              │ - dateRetourEffective│
              │ - statut        │
              ├─────────────────┤
              │ + calculerFraisRetard()│
              │ + estEnRetard() │
              └─────────────────┘
```

#### Étape 3: Règles métier formalisées

```
RG-001: Limite d'emprunts simultanés
  QUAND Adhérent.empruntsActifs.count() >= 5
  ALORS Refuser nouvel emprunt

RG-002: Durée d'emprunt
  Emprunt.dateRetourPrévue = Emprunt.dateEmprunt + 21 jours

RG-003: Frais de retard
  QUAND dateActuelle > Emprunt.dateRetourPrévue
  ALORS frais = (dateActuelle - dateRetourPrévue) * 0.50 CHF/jour

RG-004: Blocage pour impayés
  QUAND Adhérent.fraisImpayés > 0
  ALORS Adhérent.peutEmprunter() = false

RG-005: Disponibilité
  Livre.estDisponible() = true
  SI ET SEULEMENT SI aucun Emprunt actif pour ce livre
```

#### Étape 4: Implémentation technique

```java
// Entity: Adhérent
@Entity
public class Adherent {
    @Id
    private Long id;
    
    private String nom;
    private String email;
    
    @Column(name = "frais_impayes")
    private BigDecimal fraisImpayes = BigDecimal.ZERO;
    
    @OneToMany(mappedBy = "adherent")
    private List<Emprunt> emprunts = new ArrayList<>();
    
    // Logique métier: RG-004
    public boolean peutEmprunter() {
        return fraisImpayes.compareTo(BigDecimal.ZERO) == 0;
    }
    
    // Logique métier: RG-001
    public int nombreEmpruntsActifs() {
        return (int) emprunts.stream()
            .filter(Emprunt::estActif)
            .count();
    }
    
    // Logique métier: Emprunter un livre
    public Emprunt emprunterLivre(Livre livre) {
        // Validation RG-004
        if (!peutEmprunter()) {
            throw new EmpruntRefuseException(
                "Impossible d'emprunter avec des frais impayés"
            );
        }
        
        // Validation RG-001
        if (nombreEmpruntsActifs() >= 5) {
            throw new EmpruntRefuseException(
                "Limite de 5 emprunts simultanés atteinte"
            );
        }
        
        // Validation disponibilité livre
        if (!livre.estDisponible()) {
            throw new LivreIndisponibleException(
                "Le livre n'est pas disponible"
            );
        }
        
        // Création de l'emprunt - RG-002
        Emprunt emprunt = new Emprunt(this, livre);
        emprunts.add(emprunt);
        
        return emprunt;
    }
}

// Entity: Emprunt
@Entity
public class Emprunt {
    @Id
    private Long id;
    
    @ManyToOne
    private Adherent adherent;
    
    @ManyToOne
    private Livre livre;
    
    private LocalDate dateEmprunt;
    private LocalDate dateRetourPrevue;
    private LocalDate dateRetourEffective;
    
    @Enumerated(EnumType.STRING)
    private StatutEmprunt statut;
    
    // Constructeur - RG-002
    public Emprunt(Adherent adherent, Livre livre) {
        this.adherent = adherent;
        this.livre = livre;
        this.dateEmprunt = LocalDate.now();
        this.dateRetourPrevue = dateEmprunt.plusDays(21);  // RG-002
        this.statut = StatutEmprunt.ACTIF;
    }
    
    public boolean estActif() {
        return statut == StatutEmprunt.ACTIF;
    }
    
    // Logique métier: RG-003
    public boolean estEnRetard() {
        return estActif() && LocalDate.now().isAfter(dateRetourPrevue);
    }
    
    // Logique métier: RG-003
    public BigDecimal calculerFraisRetard() {
        if (!estEnRetard()) {
            return BigDecimal.ZERO;
        }
        
        long joursRetard = ChronoUnit.DAYS.between(
            dateRetourPrevue, 
            LocalDate.now()
        );
        
        return BigDecimal.valueOf(joursRetard)
            .multiply(new BigDecimal("0.50"));  // 0.50 CHF/jour
    }
    
    // Retour du livre
    public void retourner() {
        this.dateRetourEffective = LocalDate.now();
        this.statut = StatutEmprunt.TERMINE;
        
        // Calcul et ajout des frais de retard
        BigDecimal frais = calculerFraisRetard();
        if (frais.compareTo(BigDecimal.ZERO) > 0) {
            adherent.ajouterFraisImpayes(frais);
        }
    }
}

// Entity: Livre
@Entity
public class Livre {
    @Id
    private Long id;
    
    private String ISBN;
    private String titre;
    private String auteur;
    
    @OneToMany(mappedBy = "livre")
    private List<Emprunt> emprunts = new ArrayList<>();
    
    // Logique métier: RG-005
    public boolean estDisponible() {
        return emprunts.stream()
            .noneMatch(Emprunt::estActif);
    }
}

// Service métier
@Service
public class BibliothequeService {
    @Autowired
    private AdherentRepository adherentRepository;
    
    @Autowired
    private LivreRepository livreRepository;
    
    @Autowired
    private EmpruntRepository empruntRepository;
    
    @Transactional
    public EmpruntDTO emprunterLivre(Long adherentId, Long livreId) {
        Adherent adherent = adherentRepository.findById(adherentId)
            .orElseThrow(() -> new AdherentNotFoundException(adherentId));
        
        Livre livre = livreRepository.findById(livreId)
            .orElseThrow(() -> new LivreNotFoundException(livreId));
        
        // La logique métier est dans le domaine !
        Emprunt emprunt = adherent.emprunterLivre(livre);
        
        empruntRepository.save(emprunt);
        
        return EmpruntMapper.toDTO(emprunt);
    }
    
    @Transactional
    public void retournerLivre(Long empruntId) {
        Emprunt emprunt = empruntRepository.findById(empruntId)
            .orElseThrow(() -> new EmpruntNotFoundException(empruntId));
        
        emprunt.retourner();
        
        empruntRepository.save(emprunt);
    }
}
```

**Points clés de la traduction métier → technique:**

✅ **Les règles métier sont dans le modèle de domaine** (pas dans les services)
✅ **Le vocabulaire métier est préservé** (Adhérent, Emprunt, pas User, Borrow)
✅ **Les contraintes sont validées au plus tôt** (dans les méthodes métier)
✅ **Le code est testable** (logique isolée, pas de dépendances)
✅ **La cohérence est garantie** (via les invariants de l'Aggregate)

### 1.4.4 Collaboration entre analystes métier et développeurs

**Ateliers de spécification par l'exemple (Specification by Example)**

```
Scénario: Emprunter un livre avec frais impayés

ÉTANT DONNÉ:
  - Un adhérent "Jean Dupont" avec 5 CHF de frais impayés
  - Un livre "Clean Code" disponible

QUAND:
  - Jean Dupont tente d'emprunter "Clean Code"

ALORS:
  - L'emprunt est refusé
  - Un message d'erreur indique: "Impossible d'emprunter avec des frais impayés"
  - Le livre reste disponible
```

**Avantages:**
- Exemples concrets et testables
- Compréhension commune
- Base pour tests d'acceptation automatisés (Cucumber, SpecFlow)

**Processus collaboratif:**

```
1. Atelier d'exemple (3 Amigos)
   ├─ Product Owner (métier)
   ├─ Développeur (technique)
   └─ Testeur (qualité)
   
2. Identification des scénarios
   ├─ Happy path (cas nominal)
   ├─ Cas d'erreur
   └─ Cas limites

3. Formalisation en Gherkin
   Given/When/Then

4. Implémentation
   ├─ Tests automatisés
   └─ Code de production

5. Validation
   ├─ Démo au métier
   └─ Ajustements si nécessaire
```

---

## 📝 Résumé du chapitre

### Points clés à retenir

✅ Le **cycle de vie logiciel** structure le développement de la conception à la maintenance

✅ Différents **modèles** existent: Cascade, Cycle en V (rigides), Spirale, Itératif (flexibles)

✅ Le choix du modèle dépend du **contexte**: stabilité des exigences, risques, contraintes

✅ Les **exigences** (fonctionnelles et non fonctionnelles) doivent être claires, mesurables, traçables

✅ Le **cahier des charges** (SRS) formalise les exigences et sert de contrat

✅ Le **Domain-Driven Design** met le métier au cœur de la conception technique

✅ La **collaboration** métier-IT est essentielle pour un produit réussi

### Compétences acquises

Après ce chapitre, vous devez être capable de:

- ✅ Expliquer les différentes phases d'un projet logiciel
- ✅ Choisir un modèle de cycle de vie adapté au contexte
- ✅ Collecter et formaliser des exigences
- ✅ Rédiger un cahier des charges structuré
- ✅ Traduire des besoins métier en conception technique
- ✅ Appliquer les principes du Domain-Driven Design
- ✅ Assurer la traçabilité des exigences

---

## 💡 Exercices pratiques

### Exercice 1: Choix du cycle de vie

Pour chacun des projets suivants, justifiez le choix du modèle de cycle de vie:

1. Développement d'un logiciel de pilotage d'avion
2. Application mobile de réseau social (startup)
3. Migration d'un système bancaire legacy vers le cloud
4. Développement d'un POC pour une technologie blockchain

<details>
<summary>💡 Solution</summary>

1. **Cycle en V**: Système critique, exigences réglementaires strictes, besoin de traçabilité maximale
2. **Itératif/Agile**: Exigences évolutives, feedback utilisateur essentiel, time-to-market
3. **Modèle en spirale**: Risques techniques élevés, besoin de validation progressive
4. **Prototypage rapide**: Exploration technologique, incertitudes, validation de concept
</details>

### Exercice 2: Rédaction d'exigences

Transformez les demandes floues suivantes en exigences SMART:

1. "L'application doit être rapide"
2. "Le système doit être sécurisé"
3. "L'interface doit être intuitive"

<details>
<summary>💡 Solution</summary>

1. **EX-PERF-001**: Le temps de chargement de la page d'accueil doit être inférieur à 2 secondes pour 95% des utilisateurs sur une connexion 4G
2. **EX-SEC-001**: Toutes les communications entre le client et le serveur doivent utiliser TLS 1.3 minimum
   **EX-SEC-002**: Les mots de passe doivent être hashés avec bcrypt (facteur de coût minimum 12)
3. **EX-UTIL-001**: Un nouvel utilisateur doit pouvoir effectuer sa première action métier (ex: créer un document) en moins de 3 clics après connexion
   **EX-UTIL-002**: L'interface doit être conforme aux critères WCAG 2.1 niveau AA
</details>

### Exercice 3: Modélisation métier

Pour un système de réservation de restaurant en ligne:

1. Listez 5 exigences fonctionnelles
2. Listez 3 exigences non fonctionnelles
3. Identifiez les entités principales du domaine
4. Définissez 2 règles métier importantes

<details>
<summary>💡 Solution (exemple)</summary>

**Exigences fonctionnelles:**
- EX-FUNC-001: Le client doit pouvoir rechercher un restaurant par nom, ville ou type de cuisine
- EX-FUNC-002: Le client doit pouvoir réserver une table en spécifiant date, heure et nombre de convives
- EX-FUNC-003: Le système doit envoyer un email de confirmation de réservation
- EX-FUNC-004: Le restaurant doit pouvoir visualiser ses réservations par jour
- EX-FUNC-005: Le restaurant doit pouvoir confirmer ou annuler une réservation

**Exigences non fonctionnelles:**
- EX-PERF-001: Le système doit supporter 1000 réservations simultanées
- EX-SEC-001: Les données de paiement doivent être conformes PCI-DSS
- EX-DISP-001: Le système doit avoir une disponibilité de 99.5%

**Entités:**
- Restaurant
- Client
- Réservation
- Table
- Créneau horaire

**Règles métier:**
- RG-001: Une réservation ne peut être effectuée que si au moins une table est disponible pour le créneau demandé
- RG-002: Une réservation non confirmée par le restaurant dans les 2 heures est automatiquement annulée
</details>

---

## 📚 Pour aller plus loin

### Lectures recommandées

- 📖 **"Software Engineering"** - Ian Sommerville (référence sur les cycles de vie)
- 📖 **"Domain-Driven Design"** - Eric Evans (la bible du DDD)
- 📖 **"User Stories Applied"** - Mike Cohn (gestion agile des exigences)
- 📖 **"Writing Effective Use Cases"** - Alistair Cockburn

### Normes et standards

- **IEEE 830-1998**: Pratiques recommandées pour les spécifications d'exigences logicielles
- **ISO/IEC 12207**: Processus du cycle de vie du logiciel
- **SWEBOK**: Software Engineering Body of Knowledge

### Outils

- **Gestion d'exigences**: Jira, Azure DevOps, IBM DOORS, Polarion
- **Modélisation**: Enterprise Architect, Visual Paradigm, StarUML
- **Prototypage**: Figma, Adobe XD, Balsamiq, Sketch
- **Collaboration**: Miro, Mural (pour Event Storming)

---

[⬅️ Retour à la Partie I](./README.md) | [➡️ Chapitre 2: Analyse des besoins](./chapitre-2-analyse-besoins-specifications.md)

