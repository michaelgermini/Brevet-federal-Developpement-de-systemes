# Guide de contribution

Merci de votre intérêt pour contribuer au manuel de préparation au Brevet fédéral Développement de systèmes ! Ce guide vous aidera à contribuer efficacement.

---

## 📋 Table des matières

- [Code de conduite](#code-de-conduite)
- [Comment contribuer](#comment-contribuer)
- [Types de contributions](#types-de-contributions)
- [Processus de contribution](#processus-de-contribution)
- [Conventions](#conventions)
- [Suggestions et rapports de problèmes](#suggestions-et-rapports-de-problèmes)

---

## 🤝 Code de conduite

Ce projet adhère à un code de conduite simple : **soyez respectueux et constructif**.

- ✅ Respectez les autres contributeurs
- ✅ Acceptez les critiques constructives
- ✅ Focalisez sur ce qui est meilleur pour la communauté
- ✅ Faites preuve d'empathie envers les autres

---

## 💡 Comment contribuer

### Vous pouvez contribuer de plusieurs façons :

1. **Signaler des erreurs** - Typos, erreurs factuelles, liens cassés
2. **Améliorer le contenu** - Clarifications, exemples supplémentaires
3. **Ajouter du contenu** - Nouveaux exemples, exercices, ressources
4. **Traduire** - Versions en d'autres langues
5. **Réviser** - Relecture et feedback sur les PRs existantes

---

## 📝 Types de contributions

### ✍️ Corrections mineures (Typos, liens cassés)

Pour les petites corrections, vous pouvez :
1. Cliquer sur "Edit" directement sur GitHub
2. Faire la modification
3. Proposer la pull request

### 📖 Améliorations de contenu

Pour les améliorations substantielles :
1. Ouvrir d'abord une [issue](https://github.com/michaelgermini/Brevet-federal-Developpement-de-systemes/issues/new) pour discussion
2. Attendre feedback avant de travailler
3. Suivre le processus complet de contribution

### 🆕 Nouveau contenu

Pour ajouter du nouveau contenu (nouveau chapitre, section, exercices) :
1. **Obligatoire** : Ouvrir une issue pour discuter de l'ajout
2. Expliquer la valeur ajoutée et le plan
3. Attendre approbation avant de commencer
4. Suivre le format et style existants

---

## 🔄 Processus de contribution

### Étape 1 : Fork et Clone

```bash
# Fork le repository sur GitHub (bouton "Fork")

# Cloner votre fork
git clone https://github.com/VOTRE-USERNAME/Brevet-federal-Developpement-de-systemes.git
cd Brevet-federal-Developpement-de-systemes

# Ajouter le repository original comme upstream
git remote add upstream https://github.com/michaelgermini/Brevet-federal-Developpement-de-systemes.git
```

### Étape 2 : Créer une branche

```bash
# Mettre à jour votre main
git checkout main
git pull upstream main

# Créer une branche pour votre contribution
git checkout -b feature/amelioration-chapitre-5

# Exemples de noms de branches :
# - fix/typo-chapitre-3
# - docs/ajout-exemples-sql
# - feature/exercices-chapitre-7
# - translate/english-chapter-1
```

### Étape 3 : Effectuer vos modifications

- Éditez les fichiers markdown
- Testez que les liens fonctionnent
- Vérifiez l'orthographe
- Respectez le format existant

### Étape 4 : Commit

```bash
# Ajouter les fichiers modifiés
git add .

# Commit avec message descriptif
git commit -m "docs: amélioration exemples SQL chapitre 9"

# Utilisez les préfixes conventionnels :
# - docs: pour documentation
# - fix: pour corrections
# - feat: pour nouvelles fonctionnalités
# - refactor: pour restructuration
# - chore: pour maintenance
```

### Étape 5 : Push et Pull Request

```bash
# Pusher vers votre fork
git push origin feature/amelioration-chapitre-5

# Aller sur GitHub et créer une Pull Request
# Remplir le template de PR avec :
# - Description des changements
# - Motivation
# - Screenshots (si applicable)
```

### Étape 6 : Revue et itération

- Un mainteneur reviewera votre PR
- Des changements peuvent être demandés
- Effectuez les modifications demandées
- Une fois approuvée, la PR sera mergée !

---

## 📐 Conventions

### Format Markdown

```markdown
# Titre niveau 1 (réservé aux titres de chapitre)
## Titre niveau 2 (sections principales)
### Titre niveau 3 (sous-sections)

**Texte en gras** pour emphase
*Texte en italique* pour termes techniques

Code inline: `variable` ou `functionName()`

Bloc de code:
\```javascript
const example = 'code';
\```

Liste à puces:
- Item 1
- Item 2

Liste numérotée:
1. Premier
2. Deuxième

> Citation ou note importante

✅ Checkmark pour points positifs
❌ Cross pour points négatifs
⚠️  Warning pour attention
```

### Style d'écriture

- **Ton** : Professionnel mais accessible
- **Phrases** : Courtes et claires
- **Exemples** : Concrets et testés
- **Code** : Bien formaté avec commentaires explicatifs

### Organisation des fichiers

```
partie-X-nom/
├── README.md
├── chapitre-Y-titre.md
└── chapitre-Z-titre.md

Nommage:
- Tout en minuscules
- Tirets pour séparer les mots
- Pas d'espaces, caractères spéciaux
```

### Exemples de code

```javascript
// ✅ Bon exemple - Clair et commenté
function calculateTotal(items) {
    // Calculer le total en additionnant prix × quantité
    return items.reduce((sum, item) => 
        sum + (item.price * item.quantity), 0
    );
}

// ❌ Mauvais exemple - Pas clair
function calc(i) {
    return i.reduce((s,x)=>s+x.p*x.q,0);
}
```

---

## 🐛 Signaler des problèmes

### Erreur trouvée ?

Ouvrez une [issue](https://github.com/michaelgermini/Brevet-federal-Developpement-de-systemes/issues/new) en incluant :

```markdown
## Type de problème
[Erreur factuelle | Typo | Lien cassé | Code incorrect | Autre]

## Localisation
- **Fichier:** partie-X-nom/chapitre-Y-titre.md
- **Section:** [Titre de la section]
- **Ligne:** [Numéro approximatif]

## Description
[Description claire du problème]

## Correction proposée (optionnel)
[Si vous avez une suggestion de correction]

## Contexte additionnel
[Toute information utile]
```

### Suggestion d'amélioration ?

```markdown
## Suggestion
[Description de votre suggestion]

## Motivation
[Pourquoi cette amélioration serait utile]

## Implémentation proposée (optionnel)
[Comment vous l'implémenteriez]

## Alternatives considérées
[Autres approches envisagées]
```

---

## ✅ Checklist avant de soumettre une PR

- [ ] Mon code/texte suit le style du projet
- [ ] J'ai vérifié l'orthographe et la grammaire
- [ ] J'ai testé que tous les liens fonctionnent
- [ ] Les exemples de code sont fonctionnels
- [ ] J'ai mis à jour l'INDEX.md si ajout de sections
- [ ] Mon commit suit les conventions (docs/fix/feat)
- [ ] J'ai vérifié qu'il n'y a pas de doublons avec PRs existantes
- [ ] Ma PR a un titre et une description clairs

---

## 🎯 Priorités actuelles

Contributions particulièrement appréciées dans ces domaines :

1. **Exercices pratiques** - Cas concrets avec solutions
2. **Diagrammes** - Amélioration visuelle des concepts
3. **Exemples multi-langages** - Plus de langages (C#, Go, Rust)
4. **Ressources récentes** - Outils et frameworks 2024-2025
5. **Traduction** - Version anglaise du manuel
6. **Vidéos** - Tutoriels vidéo pour concepts complexes

---

## 📚 Ressources pour contributeurs

- **Markdown Guide** : [markdownguide.org](https://www.markdownguide.org)
- **Conventional Commits** : [conventionalcommits.org](https://www.conventionalcommits.org)
- **Writing Style** : Suivre le style des chapitres existants

---

## ❓ Questions ?

Si vous avez des questions sur comment contribuer :
- Ouvrez une [issue](https://github.com/michaelgermini/Brevet-federal-Developpement-de-systemes/issues/new) avec le label `question`
- Contactez : michael@germini.info

---

**Merci pour votre contribution à l'amélioration de ce manuel ! 🙏**

Ensemble, nous pouvons créer la meilleure ressource pour la préparation au Brevet fédéral.

