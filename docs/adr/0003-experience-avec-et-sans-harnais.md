# Mesurer le harnais : le même prompt avec et sans

Le cours ne se contente pas d'affirmer que le harnais améliore le code généré : il le fait mesurer. À partir d'un même commit du fil rouge, deux branches :

- `nu` : le code seul, sans `AGENTS.md`, sans outils de qualité dans `composer.json`, sans tests ;
- `outille` : le même code avec le harnais construit pendant les séances.

Un **prompt d'expérience**, versionné dans le dépôt du fil rouge, est donné tel quel au même agent et au même modèle sur chaque branche ; `git worktree` permet d'avoir les deux branches côte à côte. Le résultat est jugé par des **tests d'acceptation cachés**, écrits par l'enseignant et gardés hors du dépôt des étudiants, puis par `composer check` au niveau maximal de la branche outillée. On relève aussi les dépendances ajoutées, les failles repérées, le nombre d'itérations de l'agent et le temps passé.

Un LLM ne produit pas deux fois le même résultat : un essai par branche ne prouve rien. La moitié des binômes travaille sur `nu`, l'autre sur `outille`, et le dépouillement agrège les résultats de la classe (7 à 8 mesures par branche). Cela divise aussi par deux la consommation du modèle. L'agent doit pouvoir lancer des commandes : c'est la boucle « modifier, lancer les capteurs, se corriger » qui fait l'écart, une simple complétion de code n'en montrerait presque pas.

Le prompt doit être assez réaliste et piégeux pour que le harnais compte (une règle métier subtile, une modification qui casse un comportement existant) : un CRUD simple réussirait sur les deux branches.

**Calendrier** : une expérience de référence en séance 1 (premier prompt sur le projet brut, seule la branche `nu` existe), l'expérience complète en séance 6 (même prompt, ou un second de difficulté comparable, sur les deux branches), et de courtes démonstrations entre les deux quand le temps le permet. Seuls deux prompts complets sont préparés, car chacun demande ses propres tests d'acceptation cachés.
