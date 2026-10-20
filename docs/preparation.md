# Préparation des séances

Ce que l'enseignant doit préparer en dehors des slides. Les décisions correspondantes sont dans `docs/adr/`.

## Avant la séance 1 (05/11/2026)

- [ ] **Fil rouge `helpdesk`** (ADR 0002) : application Symfony 8 / PHP 8.4+, 15 à 25 classes, dette volontaire, règles métier non évidentes. Essayer d'abord de la faire écrire par un agent ; si la dette n'est pas tenue, introduire les défauts à la main.
- [ ] **Catalogue de dette**, hors du dépôt des étudiants : chaque défaut, la séance où il apparaît, l'outil qui le détecte. Vérifier que chaque séance a de la matière (formatage, idiomes datés pour Rector, erreurs PHPStan, violations de couches pour Deptrac, règles métier à tester).
- [ ] **Tag `seance-1`** sur le dépôt du fil rouge, et branche `nu`.
- [ ] **Premier prompt d'expérience**, versionné dans le dépôt du fil rouge, et ses **tests d'acceptation cachés**, hors du dépôt (ADR 0003).
- [ ] **Accès au modèle** pour l'expérience de référence : à défaut de la décision de l'ADR 0004, démonstration par l'enseignant.
- [ ] Quiz de positionnement.

## Avant la séance 2 (05/11/2026)

- [ ] Tag `seance-2` : état attendu du fil rouge en fin de séance 1.

## Entre novembre 2026 et mai 2027

- [ ] **Essai des modèles** (ADR 0004) : lancer le prompt d'expérience sur les deux branches avec Devstral Small 2 (Mistral), Claude Haiku 5.5 et Claude Sonnet 5.5 dans VS Code en BYOK. Relever : réussite des tests cachés, appels d'outils corrects, consommation réelle par session. Décider du fournisseur et mettre à jour l'ADR 0004.
- [ ] **Créer le compte du fournisseur retenu tôt** et l'utiliser : chez Anthropic, une nouvelle organisation démarre au niveau Evaluation, aux limites réduites.
- [ ] Vérifier les **limites de débit** du compte pour une quinzaine de sessions d'agent simultanées ; demander une hausse au support si besoin.
- [ ] Vérifier les **conditions d'utilisation des données** du fournisseur (entraînement sur les requêtes).
- [ ] **Second prompt d'expérience** (si l'expérience de la séance 6 n'utilise pas le premier) et ses tests d'acceptation cachés.
- [ ] Construire la **branche `outille`** de référence : le harnais complet attendu en fin de séance 5 (`composer check`, CI, `AGENTS.md`), pour vérifier que l'expérience fonctionne.
- [ ] Tags `seance-3` à `seance-6`.

## En avril 2027

- [ ] Revérifier la liste « à revérifier » d'`AGENTS.md` : conditions de Copilot Student, BYOK de VS Code, prix et plafonds des fournisseurs, Composer (`minimum-release-age` livré ?), Symfony AI Mate, outils de spec-driven development.

## Juste avant la séance 6 (28/05/2027)

- [ ] Créer un **workspace par binôme** chez le fournisseur, avec sa clé et un plafond mensuel ; préparer la distribution des clés.
- [ ] Rappeler aux étudiants de configurer le modèle en BYOK dans VS Code, et de ne pas compter sur les crédits de Copilot Student pour le mode agent.
- [ ] Préparer la grille de dépouillement de l'expérience : tests cachés réussis, résultat de `composer check`, dépendances ajoutées, failles, itérations, temps, modèle utilisé.
