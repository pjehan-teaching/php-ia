# Cours Développement PHP assisté par IA

Vocabulaire du cours. Les noms d'outils, de commandes et de fichiers restent en anglais (`composer check`, `AGENTS.md`, baseline PHPStan) ; le reste du discours est en français.

## Agent et harnais

**Agent** :
Assistant IA qui enchaîne lui-même les actions (lire des fichiers, les modifier, lancer des commandes) pour accomplir une tâche, ici GitHub Copilot en mode agent dans VS Code.
_Avoid_ : copilote, assistant, IA (pour désigner l'outil)

**Modèle** :
LLM qui produit les réponses de l'agent, choisi indépendamment de l'agent grâce au BYOK.
_Avoid_ : IA, moteur

**Harnais** :
Ensemble des guides et des capteurs que le développeur construit autour d'un agent pour qu'il produise du code de qualité.
_Avoid_ : harness, garde-fous, outillage (seul)

**Guide** :
Élément du harnais qui oriente l'agent avant qu'il agisse : spécification, `AGENTS.md`, conventions, skills.
_Avoid_ : feedforward, consigne, règle (seul)

**Capteur** :
Élément du harnais qui détecte une erreur après l'action de l'agent, pour qu'il se corrige : test, linter, analyse statique, revue. Un capteur est **déterministe** (même résultat à chaque exécution) ou **inférentiel** (revue par un modèle).
_Avoid_ : sensor, feedback, contrôle

**Hook d'agent** :
Commande que l'agent exécute automatiquement à un moment de son travail (après une modification de fichier, avant une commande), configurée dans l'agent.
_Avoid_ : hook (seul, ambigu avec le hook Git)

**Hook Git** :
Script exécuté par Git à une étape du cycle de commit (`pre-commit`, `pre-push`).
_Avoid_ : hook (seul)

## Méthode

**Spec-driven development** :
Démarche qui fait écrire une spécification validée avant tout code, puis la donne à l'agent comme guide.
_Avoid_ : développement par la spec, SDD (sans l'avoir défini)

**`composer check`** :
Commande unique qui lance tous les capteurs déterministes rapides du projet ; chaque séance y ajoute son outil, et `AGENTS.md` la désigne à l'agent.
_Avoid_ : script qualité, pipeline (réservé à la CI)

## Qualité du code

**Dette technique** :
Coût futur des raccourcis pris dans le code, qui rend chaque modification plus lente et plus risquée.
_Avoid_ : code legacy, mauvais code

**Code smell** :
Symptôme visible dans le code qui signale un probable problème de conception (méthode trop longue, duplication…).
_Avoid_ : mauvaise odeur, anti-pattern (qui désigne une solution type à éviter)

**Baseline** :
Liste des erreurs existantes qu'un outil d'analyse ignore, pour imposer son niveau d'exigence au seul code nouveau.
_Avoid_ : ligne de base, exceptions

## Fil rouge

**Fil rouge** :
Application Symfony de support (`helpdesk`), fournie avec une dette technique volontaire, que les étudiants assainissent de séance en séance.
_Avoid_ : projet, TP, application exemple

**Ticket** :
Demande de support du fil rouge, avec un titre, une description, un statut, une priorité et un éventuel assigné.
_Avoid_ : issue, incident

**Catalogue de dette** :
Liste, tenue par l'enseignant, des défauts introduits volontairement dans le fil rouge, chacun associé à l'outil qui le détecte.
_Avoid_ : corrigé, liste des bugs

## Expérience

**Expérience avec et sans harnais** :
Comparaison du résultat d'un même prompt donné au même agent et au même modèle sur la branche nue et sur la branche outillée.
_Avoid_ : test A/B, benchmark

**Branche nue** :
Branche `nu` du fil rouge : le code seul, sans `AGENTS.md`, sans outils de qualité ni tests.
_Avoid_ : branche vide, branche sans IA

**Branche outillée** :
Branche `outille` du fil rouge : le même code avec le harnais construit pendant les séances.
_Avoid_ : branche IA, branche qualité

**Prompt d'expérience** :
Demande de modification du fil rouge, versionnée dans le dépôt, donnée telle quelle à l'agent sur les deux branches.
_Avoid_ : consigne, exercice

**Tests d'acceptation cachés** :
Tests écrits par l'enseignant pour un prompt d'expérience, gardés hors du dépôt des étudiants, qui jugent le résultat de l'agent sans qu'il les ait vus.
_Avoid_ : tests secrets, tests de l'enseignant

## Risques du code généré

**Slopsquatting** :
Attaque qui consiste à publier un paquet malveillant sous un nom que les LLM inventent régulièrement, pour qu'un développeur ou un agent l'installe.
_Avoid_ : typosquatting (qui exploite les fautes de frappe), hallucination de paquet (qui désigne le phénomène, pas l'attaque)

**Faux test** :
Test qui passe sans vérifier le comportement attendu (assertion absente ou triviale, comportement simulé), souvent produit par un agent pour faire passer sa modification.
_Avoid_ : test inutile, test cassé
