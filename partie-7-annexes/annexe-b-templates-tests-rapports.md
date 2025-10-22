# Annexe B : Templates de tests et rapports

Cette annexe fournit des modèles prêts à l'emploi pour la gestion des tests et la création de rapports de qualité.

---

## B.1 Plan de test (Test Plan)

```markdown
# Plan de Test - [Nom du Projet]

## Identification
- **ID:** TP-001
- **Version:** 1.0
- **Date:** YYYY-MM-DD
- **Auteur:** [Nom]
- **Statut:** [Draft | Review | Approved]

## Vue d'ensemble

### Objectif
[Décrire l'objectif général des tests]

### Portée
[Couvrir quoi, ne pas couvrir quoi]

### Références
- Document de spécifications: [lien/ID]
- Cahier des charges: [lien/ID]
- Architecture: [lien/ID]

## Environnements de test

### Environnement de développement
- **Serveur:** [URL/IP]
- **Base de données:** [Type + version]
- **Outils:** [Liste]
- **Données:** [Type de données de test]

### Environnement de staging
- **Serveur:** [URL/IP]
- **Base de données:** [Type + version]
- **Outils:** [Liste]
- **Données:** Copie anonymisée production

### Environnement de production
- **Serveur:** [URL/IP]
- **Accès:** [Comment accéder pour tests]

## Stratégie de test

### Niveaux de test
1. **Tests unitaires:** [Outils, couverture cible]
2. **Tests d'intégration:** [Outils, périmètre]
3. **Tests système:** [Outils, scénarios]
4. **Tests d'acceptation:** [Outils, critères]

### Types de test
- **Fonctionnels:** [Méthodes]
- **Non fonctionnels:** [Performance, sécurité, etc.]
- **Régression:** [Automatisation]
- **Exploratoire:** [Sessions dédiées]

### Critères d'entrée/sortie
#### Critères d'entrée (quand commencer les tests)
- Code compilable sans erreurs
- Tests unitaires passant (>80% couverture)
- Environnements disponibles
- Données de test prêtes

#### Critères de sortie (quand arrêter les tests)
- Tous les tests critiques passant
- Bugs bloquants corrigés
- Métriques de qualité atteintes
- Sign-off des stakeholders

## Ressources

### Équipe de test
- **Test Manager:** [Nom]
- **Testeurs:** [Liste]
- **Développeurs:** [Support]
- **Business Analyst:** [Validation métier]

### Outils
- **Gestion des tests:** [Jira, TestRail, etc.]
- **Automatisation:** [Selenium, Cypress, etc.]
- **Performance:** [JMeter, k6, etc.]
- **Rapport:** [Allure, ReportPortal, etc.]

## Planning

### Phase 1: Préparation (Semaine 1-2)
- [ ] Environnements configurés
- [ ] Jeux de données créés
- [ ] Scripts d'automatisation développés
- [ ] Formation équipe

### Phase 2: Exécution (Semaine 3-8)
- [ ] Tests unitaires quotidiens
- [ ] Tests d'intégration hebdomadaires
- [ ] Tests système bi-hebdomadaires
- [ ] Tests de performance mensuels

### Phase 3: Clôture (Semaine 9)
- [ ] Rapports finaux
- [ ] Retours d'expérience
- [ ] Recommandations pour production

## Risques et mitigation

| Risque | Probabilité | Impact | Mitigation |
|--------|-------------|--------|------------|
| Environnements instables | Moyen | Élevé | Monitoring continu + backups |
| Données de test insuffisantes | Faible | Moyen | Scripts génération automatique |
| Délais serrés | Élevé | Moyen | Priorisation tests critiques |
| Compétences manquantes | Faible | Élevé | Formation + support experts |

## Métriques

### Couverture
- **Code:** > 80% lignes, > 75% branches
- **Fonctionnalités:** 100% features critiques
- **Plateformes:** [Liste cibles]

### Qualité
- **Taux de succès:** > 95%
- **Temps de résolution bugs:** < 24h critiques
- **Régression:** 0 nouveaux bugs

### Performance
- **Temps réponse:** < 2s (95th percentile)
- **Disponibilité:** > 99.5%
- **Throughput:** [Valeur cible] req/s

## Livrables

- [ ] Plan de test détaillé
- [ ] Cas de test (unitaires + manuels)
- [ ] Scripts d'automatisation
- [ ] Rapports d'exécution
- [ ] Matrice de traçabilité
- [ ] Rapport final avec recommandations

## Approbation
- **Préparé par:** [Nom]
- **Approuvé par:** [Nom]
- **Date:** YYYY-MM-DD
```

---

## B.2 Cas de test (Test Case)

```markdown
# Cas de Test - [ID]

## Identification
- **ID:** TC-001
- **Titre:** [Titre descriptif]
- **Module:** [Module testé]
- **Type:** [Unitaire | Intégration | Fonctionnel | Performance | Sécurité]
- **Priorité:** [Critique | Haute | Moyenne | Basse]
- **Complexité:** [Simple | Moyenne | Complexe]

## Description
[Description courte du scénario testé]

## Prérequis
- [ ] [Condition 1]
- [ ] [Condition 2]
- [ ] [Condition 3]

## Données de test
- **Utilisateur:** [Type d'utilisateur]
- **Données d'entrée:** [Description]
- **État initial:** [État du système]

## Étapes de test

| Étape | Action | Données | Résultat attendu |
|-------|--------|---------|------------------|
| 1 | [Action utilisateur] | [Données] | [Résultat] |
| 2 | [Action utilisateur] | [Données] | [Résultat] |
| 3 | [Action utilisateur] | [Données] | [Résultat] |

## Critères de succès
- [ ] [Critère 1]
- [ ] [Critère 2]
- [ ] [Critère 3]

## Critères d'échec
- [ ] [Erreur spécifique 1]
- [ ] [Erreur spécifique 2]

## Environnements
- [ ] Développement
- [ ] Staging
- [ ] Production

## Automatisation
- **Status:** [Manuel | Automatisé | À automatiser]
- **Script:** [Nom du script si automatisé]
- **Fréquence:** [À chaque commit | Quotidienne | Hebdomadaire]

## Historique d'exécution

| Date | Environnement | Résultat | Exécuté par | Commentaires |
|------|---------------|----------|-------------|--------------|
| YYYY-MM-DD | Staging | ✅ Pass | [Nom] | OK |
| YYYY-MM-DD | Prod | ❌ Fail | [Nom] | Bug #123 |

## Liens
- **Exigence:** REQ-001
- **User Story:** US-045
- **Bug lié:** BUG-123
- **Documentation:** [lien]

## Notes
[Informations complémentaires, edge cases, etc.]
```

---

## B.3 Rapport de bug (Bug Report)

```markdown
# Rapport de Bug - [ID]

## Identification
- **ID:** BUG-001
- **Titre:** [Titre descriptif et précis]
- **Priorité:** [Critique | Majeur | Mineur | Trivial]
- **Sévérité:** [Bloquant | Critique | Majeur | Mineur]
- **Status:** [Nouveau | En cours | Corrigé | Fermé | Rejeté]

## Environnement
- **OS:** [Windows 10 | macOS 12.1 | Ubuntu 20.04 | etc.]
- **Navigateur:** [Chrome 98 | Firefox 97 | Safari 15 | etc.]
- **Appareil:** [Desktop | Mobile | Tablet]
- **Résolution:** [1920x1080 | 375x667 | etc.]
- **Version app:** [v1.2.3]
- **Environnement:** [Développement | Staging | Production]

## Description
[Description claire et concise du problème]

### Étapes pour reproduire
1. [Étape 1 détaillée]
2. [Étape 2 détaillée]
3. [Étape 3 détaillée]
4. [Résultat observé]

### Résultat attendu
[Ce qui devrait se passer normalement]

### Résultat actuel
[Ce qui se passe réellement]

## Captures d'écran / Logs

### Screenshots
![Screenshot 1](path/to/screenshot1.png)
![Screenshot 2](path/to/screenshot2.png)

### Logs applicatifs
```
[2024-01-15 14:30:25] ERROR: NullPointerException at UserService.java:45
    at com.example.UserService.getUser(UserService.java:45)
    at com.example.UserController.getUser(UserController.java:32)
```

### Logs console
```
TypeError: Cannot read property 'name' of undefined
    at UserProfile.js:23
    at render (UserProfile.js:45)
```

### Logs réseau (API calls)
```json
POST /api/users HTTP/1.1
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "123"
}

HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "error": "Internal Server Error",
  "message": "Validation failed",
  "details": "Email format invalid"
}
```

## Analyse

### Cause racine suspectée
[Analyse du problème, hypothèses sur l'origine]

### Impact
- **Utilisateurs affectés:** [Nombre ou %]
- **Fonctionnalités impactées:** [Liste]
- **Business impact:** [Révenue perdue, satisfaction client, etc.]

## Reproduction
- **Taux de reproduction:** [Toujours | Souvent | Rarement | Impossible]
- **Conditions spécifiques:** [Navigateur, données, timing, etc.]

## Tentatives de contournement
[Workarounds temporaires pour les utilisateurs]

## Informations système
- **User Agent:** [Mozilla/5.0 ...]
- **IP Address:** [anonymisée si nécessaire]
- **Session ID:** [si applicable]
- **Timestamp:** [2024-01-15 14:30:25 UTC]

## Assignation
- **Rapporté par:** [Nom]
- **Assigné à:** [Nom du développeur]
- **Review par:** [Nom du QA]

## Historique

### 2024-01-15 14:35 [Rapporteur]
Bug créé avec captures d'écran et logs.

### 2024-01-15 15:00 [Développeur]
Reproduit le bug en local. Cause identifiée: validation email incorrecte.

### 2024-01-15 16:30 [Développeur]
Fix implémenté. Testé avec les données du rapport.

### 2024-01-16 09:00 [QA]
Test de régression passé. Bug marqué comme corrigé.

## Liens
- **User Story:** US-045
- **Sprint:** Sprint 12
- **Commit fix:** [hash ou lien GitHub]
- **Tests ajoutés:** TC-123, TC-124

## Checklist de clôture
- [ ] Code corrigé
- [ ] Tests ajoutés/corrigés
- [ ] Documentation mise à jour
- [ ] Tests de régression passés
- [ ] Code review effectué
- [ ] Déploiement en production
- [ ] Communication utilisateurs (si nécessaire)
```

---

## B.4 Matrice de traçabilité (Traceability Matrix)

```markdown
# Matrice de Traçabilité - [Projet]

## Légende
- ✅ Implémenté et testé
- 🔄 En cours
- ⏳ Planifié
- ❌ Non applicable
- ❓ À vérifier

## Requirements vs Tests

| Exigence | Description | TC-001 | TC-002 | TC-003 | TC-004 | Couverture |
|----------|-------------|--------|--------|--------|--------|------------|
| RF-001   | Authentification utilisateur | ✅    | ✅    |        |        | 100%      |
| RF-002   | Gestion panier             |       | ✅    | ✅    |        | 100%      |
| RF-003   | Validation paiement        |       |       | ✅    | ✅    | 100%      |
| RF-004   | Historique commandes       | ✅    |       |       |       | 50%       |
| RNF-001  | Performance < 2s          |       |       |       | ✅    | 100%      |
| RNF-002  | Sécurité OWASP            | ✅    | ✅    | ✅    | ✅    | 100%      |

## User Stories vs Code

| User Story | Description | Classe | Méthode | Tests | Status |
|------------|-------------|--------|---------|-------|--------|
| US-001     | Login user  | UserController | login() | TC-001 | ✅    |
| US-002     | Add to cart | CartService    | addItem() | TC-002 | ✅    |
| US-003     | Checkout    | OrderService   | createOrder() | TC-003 | 🔄   |
| US-004     | Payment     | PaymentService | process() | TC-004 | ⏳   |

## Code vs Tests

| Classe/Méthode | Lignes | Tests unitaires | Coverage | Status |
|----------------|--------|-----------------|----------|--------|
| UserController.login() | 25 | 3 tests | 95% | ✅    |
| CartService.addItem() | 18 | 2 tests | 100% | ✅    |
| OrderService.createOrder() | 45 | 1 test | 60% | 🔄   |
| PaymentService.process() | 32 | 0 tests | 0% | ❌    |

## Tests vs Environnements

| Test Case | Unitaire | Intégration | Staging | Production | Status |
|-----------|----------|-------------|---------|------------|--------|
| TC-001 Login | ✅      | ✅         | ✅     | ✅        | ✅    |
| TC-002 Cart | ✅      | ✅         | 🔄     | ⏳        | 🔄    |
| TC-003 Order | ✅      | ❌         | ⏳     | ⏳        | ⏳    |
| TC-004 Payment | ❌     | ❌         | ❌     | ❌        | ❌    |

## Bugs vs Fixes

| Bug ID | Description | Root Cause | Fix Commit | Tests ajoutés | Status |
|--------|-------------|------------|------------|---------------|--------|
| BUG-001 | Login fails | Password hash | abc123 | TC-001.4 | ✅    |
| BUG-002 | Cart empty | Session lost | def456 | TC-002.3 | ✅    |
| BUG-003 | Payment timeout | API config | ghi789 | TC-004.1 | 🔄    |

## Métriques globales

### Couverture fonctionnelle
- Requirements: 85% (17/20)
- User Stories: 90% (9/10)
- Code lines: 78%
- Branches: 72%

### Couverture par environnement
- Unitaire: 95%
- Intégration: 80%
- E2E: 60%
- Performance: 40%

### Qualité
- Tests passant: 92%
- Code smells: 23 (target: < 50)
- Security issues: 0
- Performance issues: 2

## Actions requises

### Priorité haute
1. [ ] Implémenter TC-004 (Payment tests)
2. [ ] Corriger BUG-003 (Payment timeout)
3. [ ] Améliorer coverage branches > 75%

### Priorité moyenne
1. [ ] Ajouter tests E2E pour checkout
2. [ ] Automatiser tests de performance
3. [ ] Réduire code smells

### Priorité basse
1. [ ] Tests d'accessibilité
2. [ ] Tests multi-langues
3. [ ] Tests de charge avancés
```

---

## B.5 Rapport de test final (Test Summary Report)

```markdown
# Rapport de Test Final - [Projet] v[X.Y.Z]

## Informations générales
- **Période:** Du YYYY-MM-DD au YYYY-MM-DD
- **Environnements:** Développement, Staging, Production
- **Équipe:** [Noms des testeurs]
- **Outils:** [Liste des outils utilisés]

## Exécutif Summary

### Status global
- **Résultat:** [✅ PASS | ❌ FAIL | ⚠️ PASS avec réserves]
- **Critère de sortie:** [Atteint | Non atteint]
- **Recommandation:** [Go/No-Go pour production]

### Métriques clés
- **Tests exécutés:** 1,247
- **Tests passant:** 1,152 (92.4%)
- **Tests en échec:** 95 (7.6%)
- **Tests bloquants:** 23 (1.8%)
- **Coverage code:** 78%

## Détail des résultats

### Par niveau de test

#### Tests unitaires
- **Total:** 892 tests
- **Passant:** 845 (94.7%)
- **Échec:** 47 (5.3%)
- **Coverage:** 85% lignes, 78% branches
- **Temps exécution:** 45 secondes

#### Tests d'intégration
- **Total:** 234 tests
- **Passant:** 198 (84.6%)
- **Échec:** 36 (15.4%)
- **Coverage:** 72% services
- **Temps exécution:** 8 minutes

#### Tests système (E2E)
- **Total:** 89 tests
- **Passant:** 76 (85.4%)
- **Échec:** 13 (14.6%)
- **Scénarios couverts:** 94%
- **Temps exécution:** 25 minutes

#### Tests de performance
- **Total:** 32 tests
- **Passant:** 28 (87.5%)
- **Échec:** 4 (12.5%)
- **Métriques:**
  - Response time < 2s: ✅ (95th percentile: 1.2s)
  - Throughput > 100 req/s: ✅ (142 req/s)
  - Memory usage < 80%: ⚠️ (85% peak)

### Par module/fonctionnalité

| Module | Tests | Pass | Fail | Coverage | Status |
|--------|-------|------|------|----------|--------|
| Authentification | 156 | 149 | 7 | 92% | ⚠️   |
| Gestion utilisateurs | 98 | 95 | 3 | 88% | ✅   |
| Catalogue produits | 134 | 127 | 7 | 85% | ⚠️   |
| Panier d'achat | 89 | 84 | 5 | 82% | ✅   |
| Commandes | 167 | 152 | 15 | 78% | ⚠️   |
| Paiement | 123 | 98 | 25 | 65% | ❌   |
| API externe | 67 | 62 | 5 | 90% | ✅   |
| Interface admin | 89 | 85 | 4 | 88% | ✅   |

## Bugs et anomalies

### Par sévérité
- **Critique (P0):** 3 bugs (tous corrigés)
- **Majeur (P1):** 12 bugs (10 corrigés, 2 différés)
- **Mineur (P2):** 28 bugs (25 corrigés, 3 documentés)
- **Trivial (P3):** 15 bugs (8 corrigés, 7 ignorés)

### Par status
- **Corrigés:** 46 (67%)
- **Différés:** 9 (13%)
- **Rejetés:** 5 (7%)
- **Ouverts:** 10 (13%)

### Bugs bloquants identifiés
1. **BUG-001:** Erreur paiement Stripe - **CORRIGÉ**
2. **BUG-002:** Session perdue mobile - **CORRIGÉ**
3. **BUG-003:** Timeout API externe - **DIFFÉRÉ** (v2.1)
4. **BUG-004:** Validation email incorrecte - **CORRIGÉ**

## Performance et charge

### Tests de charge
- **Utilisateurs simultanés:** 1000
- **Durée:** 30 minutes
- **Résultats:**
  - Taux d'erreur: 2.1% (acceptable < 5%)
  - Latence moyenne: 850ms
  - CPU max: 75%
  - Mémoire max: 82%

### Tests de stress
- **Pic:** 2000 utilisateurs
- **Durée:** 5 minutes
- **Résultats:** Stable, récupération automatique

## Sécurité

### Tests de sécurité exécutés
- **OWASP Top 10:** ✅ 8/10 contrôles passés
- **Authentification:** ✅ Robust
- **Autorisation:** ⚠️ 2 failles mineures
- **Injection:** ✅ Protégé
- **XSS/CSRF:** ✅ Protégé
- **Configuration:** ⚠️ Headers manquants

### Recommandations sécurité
1. Ajouter CSP headers
2. Implémenter rate limiting API
3. Renforcer validation entrées
4. Audit logs sécurité

## Recommandations

### Pour le déploiement
- ✅ Fonctionnalités critiques testées
- ⚠️ Résoudre bugs P1 restants
- ✅ Performance acceptable
- ⚠️ Sécuriser configuration production

### Améliorations futures
1. **Coverage:** Atteindre 85% global
2. **Automatisation:** Plus de tests E2E
3. **Performance:** Optimiser mémoire
4. **Sécurité:** Audit complet externe

## Conclusion

Le projet est **prêt pour le déploiement** avec les réserves suivantes :
- Résoudre les 2 bugs P1 différés
- Améliorer la sécurité (headers, rate limiting)
- Monitorer les performances en production

**Recommandation finale:** ✅ **APPROUVER** le déploiement avec suivi rapproché des métriques production.

## Signatures

**Test Manager:** ____________________ Date: ________
**Product Owner:** ___________________ Date: ________
**Développeur Lead:** ________________ Date: ________
**QA Lead:** _________________________ Date: ________

## Annexes
1. Liste détaillée des bugs
2. Résultats des tests de performance
3. Rapport de sécurité détaillé
4. Métriques de couverture
5. Logs des exécutions
```

---

## B.6 Template checklist qualité

```markdown
# Checklist Qualité - [Fonctionnalité/Module]

## Code Quality
- [ ] Code respecte les standards (ESLint, SonarQube)
- [ ] Nommage cohérent (variables, fonctions, classes)
- [ ] Commentaires pertinents (quoi, pourquoi, pas comment)
- [ ] Pas de code dupliqué (DRY principle)
- [ ] Fonctions < 30 lignes, classes < 300 lignes
- [ ] Complexité cyclomatique < 10

## Tests
- [ ] Tests unitaires présents (couverture > 80%)
- [ ] Tests d'intégration pour APIs externes
- [ ] Tests E2E pour scénarios critiques
- [ ] Tests de performance (si applicable)
- [ ] Tests de sécurité (auth, input validation)
- [ ] Tests passent sur tous environnements

## Architecture
- [ ] Respect des principes SOLID
- [ ] Séparation des préoccupations
- [ ] Couplage faible, cohésion forte
- [ ] Design patterns appropriés
- [ ] Architecture documentée (ADR)

## Performance
- [ ] Temps de réponse < [X] secondes
- [ ] Pas de N+1 queries
- [ ] Cache approprié
- [ ] Optimisation base de données (index)
- [ ] Gestion mémoire (pas de leaks)

## Sécurité
- [ ] Validation des entrées (serveur + client)
- [ ] Authentification/autorisation
- [ ] Protection XSS/CSRF
- [ ] Chiffrement données sensibles
- [ ] Audit logging
- [ ] Conformité RGPD

## Accessibilité
- [ ] WCAG 2.1 niveau AA
- [ ] Support clavier
- [ ] Lecteurs d'écran
- [ ] Contraste couleurs
- [ ] Responsive design

## Documentation
- [ ] Code documenté (JSDoc, JavaDoc)
- [ ] API documentée (Swagger/OpenAPI)
- [ ] README à jour
- [ ] Guide utilisateur
- [ ] Runbook déploiement

## Compatibilité
- [ ] Navigateurs supportés (Chrome, Firefox, Safari, Edge)
- [ ] Versions mobiles (iOS 12+, Android 8+)
- [ ] APIs externes fonctionnelles
- [ ] Dépendances à jour

## Déploiement
- [ ] Scripts déploiement automatisés
- [ ] Rollback possible
- [ ] Variables d'environnement
- [ ] Secrets sécurisés
- [ ] Monitoring en place

## Sign-off
- [ ] Code review effectuée
- [ ] Tests de régression passés
- [ ] QA validation
- [ ] Product Owner acceptance
- [ ] Security review (si applicable)

**Status:** [ ] En développement  [ ] Review  [ ] Approuvé  [ ] Rejeté
**Commentaires:** ________________________________________________
__________________________________________________________________
__________________________________________________________________
```

---

[⬅️ Partie VII](./README.md) | [➡️ Annexe C](./annexe-c-exemples-planification-projet.md)

