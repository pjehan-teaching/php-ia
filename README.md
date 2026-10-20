# Développement PHP assisté par IA

## Installation

Ce slideshow utilise revealexpress 2.0, dont les sources sont attendues dans `../../revealexpress`.

```bash
git clone https://github.com/pjehan-teaching/php-ia
cd php-ia
npm install
npm run build      # ou npm run watch
npm start          # http://<ip>:3000
```

## Plan du cours

**Durée totale :** 21 heures (6 demi-journées de 3h30), dont environ deux tiers de pratique  
**Public :** développeurs PHP connaissant Symfony (contrôleurs, services, Doctrine)  
**Technologies :** PHP 8.4+, Symfony 8, Doctrine, PHP-CS-Fixer, Twig CS Fixer, Rector, PHPStan (avec `phpstan-symfony` et `phpstan-doctrine`), Deptrac, PHPUnit, Infection, hooks Git, GitHub Actions, Composer 2.10+, VS Code + GitHub Copilot (mode agent, modèle en BYOK), Symfony AI Mate  
**Fil rouge :** application Symfony de support (`helpdesk`), fournie avec une dette technique volontaire, que les étudiants assainissent de séance en séance

Le cours apprend à construire le **harnais** qui permet à un agent IA de produire du code de qualité : des **guides** qui l'orientent avant qu'il agisse, des **capteurs** qui détectent ses erreurs pour qu'il se corrige (voir [docs/adr/0001-le-harnais-comme-fil-conducteur.md](docs/adr/0001-le-harnais-comme-fil-conducteur.md)). Chaque séance ajoute un outil à la commande `composer check`, que l'agent lance pour vérifier son travail. Une expérience répétée compare le même prompt sur une branche sans outils et sur une branche outillée (voir [docs/adr/0003-experience-avec-et-sans-harnais.md](docs/adr/0003-experience-avec-et-sans-harnais.md)). Le vocabulaire employé est défini dans [CONTEXT.md](CONTEXT.md).

Les deux séances suivantes du module (découverte du protocole MCP, MCP avec Symfony) ont leur propre slideshow : [pjehan-teaching/mcp](https://github.com/pjehan-teaching/mcp).

---

## Séance 1 : Test de positionnement et qualité logicielle (05/11/2026 matin, 3h30)

### Objectifs
- Évaluer le niveau de chaque étudiant
- Comprendre pourquoi un agent IA a besoin d'un harnais, et ce qu'apporte une spécification écrite avant le code
- Identifier la dette technique et les code smells
- Appliquer les principes SOLID, KISS et DRY et les standards PSR
- Mettre en place le projet fil rouge

### Test de positionnement (20 min)
- Quiz

### Contenu théorique (75 min)
- Développer avec un agent IA : du code plausible n'est pas du code juste
- Le harnais : guides et capteurs, capteurs déterministes (tests, linters, analyse statique) et capteurs inférentiels (revue par IA)
- Spec-driven development : écrire la spécification avant le code, et les outils qui le standardisent (GitHub Spec Kit, OpenSpec, BMAD), présentés sans être appliqués
- Dette technique et code smells
- SOLID, KISS, DRY
- Standards PSR : autoloading (PSR-4), style de code (PER Coding Style)

### Mise en pratique (115 min)
- Installer le fil rouge : cloner, installer, créer la base, lancer l'application (30 min)
- Repérer les code smells du fil rouge et les classer (45 min)
- Expérience de référence : donner le premier prompt d'expérience à l'agent sur le projet brut, puis mesurer le résultat avec les tests d'acceptation cachés (40 min)

---

## Séance 2 : Linters et formatage du code (05/11/2026 après-midi, 3h30)

### Objectifs
- Configurer un formateur de code (PHP-CS-Fixer, Twig CS Fixer)
- Utiliser les linters Symfony (container, Twig, YAML)
- Moderniser du code automatiquement avec Rector
- Créer la commande `composer check`

### Contenu théorique (60 min)
- PHP-CS-Fixer : règles `@Symfony` et `@PER-CS`, mode `--dry-run`
- Twig CS Fixer
- `lint:container`, `lint:twig`, `lint:yaml` : ils détectent les services et options de configuration inventés par un agent
- Rector : ensembles de règles PHP et Symfony, aperçu avant application ; un LLM reproduit les idiomes datés de ses données d'entraînement, Rector les modernise
- Scripts Composer : la commande `composer check`, sa sortie concise, et pourquoi un agent a besoin d'une seule commande de vérification

### Mise en pratique (150 min)
- Configurer PHP-CS-Fixer et Twig CS Fixer, corriger le fil rouge
- Lancer les linters Symfony et corriger les erreurs
- Moderniser le fil rouge avec Rector, en relisant chaque changement
- Créer `composer check` avec les outils de la séance
- Démonstration : un agent corrige seul ses erreurs de style quand `composer check` échoue

---

## Séance 3 : Analyse statique (25/05/2027 matin, 3h30)

### Objectifs
- Détecter des bugs sans exécuter le code avec PHPStan
- Augmenter progressivement le niveau d'exigence (niveaux, baseline)
- Imposer des règles d'architecture avec Deptrac

### Rappel (15 min)
- Le harnais et `composer check` après six mois d'interruption ; reprise du fil rouge depuis le tag `seance-3`

### Contenu théorique (60 min)
- PHPStan : niveaux, configuration, format de sortie adapté à un agent
- Extensions `phpstan-symfony` et `phpstan-doctrine` : conteneur de services, dépôts et entités
- Baseline : imposer l'exigence au nouveau code sans corriger tout l'existant
- Deptrac : couches, règles de dépendance, ce que l'architecture interdit à l'agent de contourner

### Mise en pratique (135 min)
- Monter le niveau de PHPStan sur le fil rouge, générer une baseline
- Corriger les erreurs détectées
- Décrire les couches du fil rouge dans Deptrac et corriger les violations
- Ajouter PHPStan et Deptrac à `composer check`
- Démonstration : PHPStan attrape une méthode inventée par un agent

---

## Séance 4 : TDD et tests Symfony (27/05/2027 matin, 3h30)

### Objectifs
- Appliquer le cycle Red / Green / Refactor avec PHPUnit
- Écrire des tests unitaires et fonctionnels (WebTestCase)
- Mesurer la fiabilité des tests (couverture, mutation avec Infection)

### Contenu théorique (60 min)
- Cycle Red / Green / Refactor
- PHPUnit : tests unitaires, `KernelTestCase`, `WebTestCase`, base de test
- Couverture de code et ses limites
- Mutation testing avec Infection : MSI, mutants survivants
- Les tests comme cible pour l'agent : écrire les tests, vérifier qu'ils échouent, les commiter, puis faire implémenter sans les modifier

### Mise en pratique (150 min)
- Implémenter en TDD une règle métier du fil rouge
- Écrire les tests fonctionnels d'un parcours
- Mesurer la couverture, puis lancer Infection et renforcer les tests faibles
- Ajouter les tests à `composer check` (Infection est réservé à la CI, trop lent pour la boucle de l'agent)

---

## Séance 5 : Automatiser la qualité (27/05/2027 après-midi, 3h30)

### Objectifs
- Bloquer un commit non conforme avec des hooks Git
- Mettre en place un pipeline d'intégration continue
- Sécuriser les dépendances : détecter les dépendances vulnérables et se protéger des paquets malveillants

### Contenu théorique (60 min)
- Hooks Git : `pre-commit`, `pre-push` ; un agent peut les contourner avec `--no-verify`, d'où la CI comme barrière finale
- Pipeline GitHub Actions : installation, `composer check`, Infection
- Barrières successives : vérifications de l'agent, hook, CI, revue humaine
- `composer audit`, blocage des versions vulnérables (Composer 2.9), politique `config.policy` et blocage des malwares connus (Composer 2.10), `allow-plugins`, versions immuables sur Packagist
- Slopsquatting : les paquets inventés par les LLM, et pourquoi `composer audit` ne les détecte pas

### Mise en pratique (150 min)
- Installer un hook `pre-commit` qui lance `composer check`
- Écrire le workflow GitHub Actions du fil rouge
- Lancer `composer audit` sur une version vulnérable d'une dépendance, configurer `config.policy`
- Relire un diff de `composer.json` et `composer.lock` proposé par un agent

---

## Séance 6 : Développement assisté par IA sous contrôle (28/05/2027 matin, 3h30)

### Objectifs
- Rédiger des règles de projet pour un agent IA (`AGENTS.md`)
- Faire valider le code généré par les tests et les linters
- Identifier les risques du code généré (dépendances inventées, failles, faux tests)
- Mesurer ce que le harnais apporte

### Contenu théorique (60 min)
- `AGENTS.md` : ce qu'il doit contenir (commandes, conventions qui s'écartent des usages, pièges connus), ce qu'il ne doit pas contenir ; `CLAUDE.md` qui l'importe
- VS Code + GitHub Copilot en mode agent, modèle en BYOK, permissions
- Hooks d'agent : formater après chaque modification, exiger une validation avant `composer require`
- Symfony AI Mate : donner à l'agent l'accès au profiler, aux logs et au conteneur
- Risques du code généré : dépendances inventées, failles, faux tests ; chaque erreur récurrente devient un guide ou un capteur
- Retour sur le spec-driven development

### Mise en situation (150 min)
- Rédiger l'`AGENTS.md` du fil rouge et configurer les hooks d'agent (30 min)
- Installer Symfony AI Mate (15 min)
- Expérience avec et sans harnais : la moitié des binômes sur la branche nue, l'autre sur la branche outillée, même prompt d'expérience (60 min)
- Dépouillement collectif : tests d'acceptation cachés, `composer check`, comparaison avec l'expérience de référence de la séance 1 (30 min)
- Exercice : vérifier sur Packagist les paquets proposés par un LLM pour une tâche de niche (15 min)

---

## Évaluation

- Séance 1 : test de positionnement (quiz)
- Séance 8 (slideshow MCP) : QCM qualité du code, QCM MCP, TP à réaliser à la maison

## Ressources

- [Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html) (Birgitta Böckeler, martinfowler.com)
- [AGENTS.md](https://agents.md)
- [PHP-CS-Fixer](https://cs.symfony.com), [Twig CS Fixer](https://github.com/VincentLanglet/Twig-CS-Fixer), [Rector](https://getrector.com)
- [PHPStan](https://phpstan.org), [Deptrac](https://deptrac.github.io/deptrac/)
- [PHPUnit](https://phpunit.de), [Infection](https://infection.github.io)
- [Composer : audit et politiques de dépendances](https://getcomposer.org/doc/03-cli.md#audit)
- [Symfony AI Mate](https://symfony.com/doc/current/ai/components/mate.html)
- [Modèles de langage dans VS Code (BYOK)](https://code.visualstudio.com/docs/agent-customization/language-models)
- [GitHub Spec Kit](https://github.com/github/spec-kit), [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD)
