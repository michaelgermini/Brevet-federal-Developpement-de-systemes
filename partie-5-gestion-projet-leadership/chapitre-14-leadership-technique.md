# Chapitre 14 : Leadership technique

> **Objectif:** Développer les compétences de leadership pour diriger des équipes techniques, mentorer les développeurs et promouvoir une culture d'excellence.

---

## 📋 Table des matières

1. [Le rôle du lead developer / architecte](#141-le-rôle-du-lead-developer--architecte)
2. [Gestion de l'équipe et mentoring](#142-gestion-de-léquipe-et-mentoring)
3. [Coordination avec la direction IT et les métiers](#143-coordination-avec-la-direction-it-et-les-métiers)
4. [Culture de la qualité et innovation continue](#144-culture-de-la-qualité-et-innovation-continue)

---

## 14.1 Le rôle du lead developer / architecte

### 14.1.1 Responsabilités

**Lead Developer:**
- Guider techniquement l'équipe
- Revues de code et mentoring
- Standards de qualité
- Résolution de problèmes techniques complexes
- Interface avec Product Owner/Scrum Master

**Architecte Logiciel:**
- Vision technique long terme
- Décisions architecturales (ADRs)
- Veille technologique
- POCs et évaluations
- Documentation architecture

### 14.1.2 Compétences clés

**Techniques:**
- Expertise approfondie (backend, frontend, databases, infra)
- Capacité à résoudre problèmes complexes
- Connaissance des patterns et best practices
- Compréhension des trade-offs

**Soft skills:**
- Communication claire (technique ↔ non-technique)
- Écoute active
- Empathie
- Prise de décision
- Gestion des conflits

**Leadership:**
- Vision et stratégie
- Influence sans autorité formelle
- Encouragement et motivation
- Délégation efficace

---

## 14.2 Gestion de l'équipe et mentoring

### 14.2.1 One-on-One (1:1)

**Fréquence:** Hebdomadaire ou bi-hebdomadaire (30-60 min)

**Structure type:**

```
1. Check-in personnel (5 min)
   - Comment vas-tu?
   - Charge de travail OK?

2. Revue des tâches en cours (10 min)
   - Obstacles?
   - Besoin d'aide?

3. Discussion carrière/développement (15 min)
   - Objectifs à court/moyen terme
   - Compétences à développer
   - Formation souhaitée

4. Feedback bidirectionnel (10 min)
   - Ce qui fonctionne bien
   - Points d'amélioration

5. Actions (5 min)
   - Résumé des actions
   - Timeline

6. Questions ouvertes (5 min)
   - Espace pour tout sujet
```

**Questions efficaces:**
- "Qu'est-ce qui t'a le plus motivé cette semaine?"
- "Qu'est-ce qui t'a frustré?"
- "Comment puis-je t'aider à mieux réussir?"
- "Où te vois-tu dans 6 mois/1 an?"
- "Quelle compétence aimerais-tu développer?"

### 14.2.2 Mentoring

**Parcours d'onboarding (junior):**

```
Semaine 1: Installation et découverte
- Setup environnement
- Tour du codebase
- Lecture documentation
- Premier fix de bug simple

Semaine 2-3: Petites fonctionnalités
- Feature simple end-to-end
- Pair programming avec senior
- Première code review

Mois 2: Autonomie progressive
- Features moyennes complexité
- Participation sprint planning
- Premiers tech talks internes

Mois 3+: Contributeur à part entière
- Features complexes
- Mentorat de stagiaires
- Présentation démos
```

**Techniques de mentoring:**

**Pair Programming:**
```
Driver (junior): Code
Navigator (senior): Guide, explique, pose questions

Bénéfices:
- Transfert de connaissance immédiat
- Qualité de code améliorée
- Moins de bugs
- Confiance renforcée
```

**Code Review pédagogique:**
```
❌ "Ce code est nul"
✅ "Cette approche fonctionne, mais as-tu considéré [alternative]?
   Cela pourrait améliorer [aspect] parce que [raison]."

❌ "Fix ça"
✅ "J'ai remarqué [problème]. Peux-tu expliquer ton raisonnement?
   Je te suggère de regarder [ressource/pattern]."
```

### 14.2.3 Gestion des conflits

**Approche:**

1. **Écouter activement** les deux parties séparément
2. **Comprendre** la source du conflit (technique? Personnalité? Communication?)
3. **Faciliter** une discussion constructive
4. **Focaliser** sur les faits et solutions, pas les personnes
5. **Documenter** l'accord trouvé
6. **Suivre** l'évolution

**Exemple:**

```
Conflit: Sarah (frontend) vs Mike (backend) sur format API response

1. Écouter:
   - Sarah: Veut format plat pour performance frontend
   - Mike: Préfère format nested pour cohérence

2. Comprendre:
   - Technique: Trade-off performance vs structure
   - Communication: Pas de discussion avant implémentation

3. Faciliter:
   - Réunion commune
   - Exposer les contraintes de chacun
   - Brainstorm solutions ensemble

4. Solution trouvée:
   - Format nested par défaut
   - Query param ?flat=true pour cas performance critique
   - Documentation claire des deux formats

5. Documenter:
   - ADR créé
   - Standards API mis à jour

6. Suivre:
   - Check-in après 1 semaine
   - Collaboration améliorée
```

---

## 14.3 Coordination avec la direction IT et les métiers

### 14.3.1 Communication avec les non-techniques

**Adapter le langage:**

```
❌ Technique:
"On doit refactorer le legacy monolithe en microservices avec event sourcing
et CQRS pour améliorer la scalabilité horizontale et réduire le couplage."

✅ Business:
"Notre système actuel limite notre croissance. En le modernisant, nous pourrons:
- Ajouter de nouvelles fonctionnalités 3x plus vite
- Supporter 10x plus d'utilisateurs sans ralentissement
- Réduire les pannes de 80%
- Investissement: 6 mois, ROI estimé: 18 mois"
```

**Présentation à la direction:**

```
Structure:
1. Contexte et problème (2 min)
2. Solution proposée (3 min)
3. Bénéfices business (3 min)
4. Coûts et risques (2 min)
5. Recommandation et next steps (2 min)
6. Q&A (8 min)

Support visuel:
- Schémas simples
- Métriques business
- Timeline claire
- Budget transparent
```

### 14.3.2 Gestion des attentes

**Dire non de manière constructive:**

```
❌ "C'est impossible en 2 semaines"

✅ "Je comprends l'urgence. Pour livrer en 2 semaines, voici nos options:
   1. Version simplifiée (fonctionnalités A+B uniquement)
   2. Version complète en 6 semaines avec équipe actuelle
   3. Version complète en 3 semaines avec 2 développeurs supplémentaires
   Quelle est la priorité: délai, scope ou budget?"
```

**Expliquer la dette technique:**

```
Analogie voiture:
"La dette technique, c'est comme ne pas faire les vidanges de votre voiture.
Ça roule encore aujourd'hui, mais:
- Chaque nouveau kilomètre coûte plus cher
- Le risque de panne augmente
- Un jour, réparation coûtera 10x le prix des vidanges manquées

Notre dette technique actuelle:
- Ralentit développement de 40%
- Cause 60% des bugs production
- Proposer: 2 semaines remboursement maintenant = économie 2 mois sur l'année"
```

---

## 14.4 Culture de la qualité et innovation continue

### 14.4.1 Promouvoir les bonnes pratiques

**Code Reviews obligatoires:**
- Aucun merge sans approbation
- Checklist standardisée
- Feedback dans les 24h
- Mentalité: Apprendre ensemble, pas juger

**Tests automatisés:**
- Coverage minimum requis (ex: 80%)
- Tests dans la Definition of Done
- Tests cassés = pipeline bloqué
- Célébrer quand bugs attrapés par tests

**Documentation vivante:**
- README à jour (responsabilité de tous)
- ADRs pour décisions importantes
- Runbooks pour ops
- API documentation générée automatiquement

### 14.4.2 Innovation

**Tech Talks internes (bi-hebdomadaires):**
```
Format: 20 min présentation + 10 min Q&A

Sujets:
- Nouvelle techno utilisée
- Pattern découvert
- Retour d'expérience projet
- Veille technologique

Rotation speakers, encourage participation juniors
```

**Hack Days (trimestriels):**
```
1 jour entier pour:
- Expérimenter nouvelles technos
- Résoudre problèmes irritants
- Améliorer tooling
- Projets personnels liés au produit

Règles:
- Pas de jugement, safe to fail
- Démo en fin de journée (optionnel)
- Meilleure idée = budget implémentation
```

**20% Time / Innovation Sprints:**
```
Concept:
- 1 journée par sprint (ou 1 sprint par trimestre)
- Chacun travaille sur projet innovant de son choix
- Doit apporter valeur à l'entreprise/équipe

Exemples:
- Automatisation tâche répétitive
- POC nouvelle architecture
- Amélioration UX/DX
- Réduction dette technique
```

### 14.4.3 Reconnaissance et célébration

**Valoriser les succès:**
- Shout-outs publics (Slack, réunions)
- Wall of Fame (contributions remarquables)
- Awards techniques (meilleure PR, meilleur refactoring)
- Célébrations milestones (release majeure, 0 bug prod 1 mois)

**Culture du feedback positif:**
```
Ratio 5:1 - Pour chaque feedback correctif, 5 feedbacks positifs

Exemples:
"Excellent travail sur cette PR, ton refactoring a simplifié le code de 40%"
"Merci d'avoir pris le temps d'aider Sarah sur ce bug complexe"
"Ta présentation sur GraphQL était claire et inspirante"
```

**Échec constructif:**
```
Postmortem sans blame:
- "Qu'est-ce qui n'a pas fonctionné?" (Pas "Qui a échoué?")
- Focus sur le système, pas les individus
- Actions préventives, pas sanctions

Principe: On échoue ensemble, on apprend ensemble
```

---

## 📝 Résumé

✅ **Lead Developer:** Guide technique, mentore, maintient standards

✅ **Architecte:** Vision long terme, décisions structurantes

✅ **1:1 réguliers:** Développement carrière, feedback bidirectionnel

✅ **Mentoring:** Onboarding structuré, pair programming, code reviews pédagogiques

✅ **Communication:** Adapter langage technique/business, gérer attentes

✅ **Culture:** Promouvoir qualité, encourager innovation, célébrer succès

---

## 💡 Exercices

1. Préparer et animer un 1:1 fictif avec scenarios variés
2. Créer un plan d'onboarding pour développeur junior
3. Présenter décision technique à audience non-technique

---

## 📚 Pour aller plus loin

- 📖 **"The Manager's Path"** - Camille Fournier
- 📖 **"An Elegant Puzzle"** - Will Larson
- 📖 **"Staff Engineer"** - Will Larson
- 📖 **"Team Topologies"** - Matthew Skelton

---

[⬅️ Chapitre 13](./chapitre-13-methodologies-travail.md) | [➡️ Partie VI](../partie-6-projet-brevet/README.md)

