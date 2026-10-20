# Un fil rouge fourni, avec une dette technique volontaire

Les étudiants ne créent pas le fil rouge : ils reçoivent une application Symfony de support (`helpdesk`, gestion de tickets) écrite pour l'occasion, récente par ses versions (PHP 8.4+, Symfony 8) mais datée par son style : contrôleurs trop gros, logique métier dans les contrôleurs, peu de types, tableaux associatifs à la place d'objets, duplication, aucun test. Elle reste petite (15 à 25 classes) pour se lire en une séance, et contient des règles métier non évidentes (par exemple : un ticket fermé ne se réassigne pas, la priorité dépend du client) sur lesquelles un agent sans harnais doit trébucher.

Un projet créé de zéro aurait été propre : les outils n'y auraient presque rien trouvé, l'objectif « identifier la dette technique » de la séance 1 n'aurait pas eu de matière, et un agent réussit bien sur du code neuf, ce qui aurait effacé l'écart mesuré par l'expérience (ADR 0003). Un code commun à toute la classe permet aussi de comparer et d'agréger les résultats des binômes, et de reprendre le cours en mai 2027 après six mois d'interruption sans dépendre de l'état du dépôt de chaque étudiant.

Le domaine des tickets fait le lien avec le slideshow MCP (séances 7 et 8), qui part de son propre projet `tickets-raw` ; le nom `helpdesk` évite de confondre les deux projets.

**Conséquences** :

- L'enseignant tient un **catalogue de dette**, hors du dépôt des étudiants : chaque défaut introduit, et l'outil qui le détecte. Il sert de corrigé et garantit que chaque séance a de quoi faire.
- Le dépôt du fil rouge porte un tag par séance (`seance-1`, `seance-2`…) : un étudiant en retard repart d'un état propre, et les expériences partent toujours du même commit.
- Le fil rouge est écrit avec l'aide d'un agent, ce qui reste à tester : un LLM a tendance à écrire du code propre et à corriger la dette qu'on lui demande d'introduire. Si l'essai échoue, l'agent écrit la base et l'enseignant introduit la dette à la main.
