# VS Code + Copilot en mode agent, avec un modèle commun en BYOK

L'agent du cours est **GitHub Copilot en mode agent dans VS Code**, déjà utilisé dans le slideshow MCP. Il lit `AGENTS.md` nativement, propose des hooks d'agent et des permissions, et accepte un modèle externe en **BYOK** (« bring your own key ») : d'après la documentation de VS Code (octobre 2026), un modèle BYOK fonctionne en mode agent, sans plan Copilot et sans compte GitHub, à condition qu'il sache appeler des outils. Les complétions de code, elles, ne passent pas par un modèle BYOK.

Le plan gratuit **Copilot Student** ne suffit pas pour l'expérience (ADR 0003), d'après sa situation en octobre 2026 :

- 200 crédits IA par mois depuis le 1er juin 2026 (chiffre de l'annonce de GitHub, discussion #189268 ; la documentation ne donne pas de montant), soit environ 2 $ d'usage, remis à zéro le 1er de chaque mois. Une session d'agent de 20 à 30 minutes peut épuiser une grande partie du quota, et les séances 3 à 6 tombent du 25 au 28 mai 2027, en fin de mois.
- Sélection automatique du modèle uniquement depuis le 24 juin 2026 : les deux branches pourraient être traitées par des modèles différents, ce qui fausserait la comparaison.

Copilot Student reste utile pour les complétions de code, illimitées et gratuites. Pour le mode agent, toute la classe utilise **le même modèle, avec une clé par binôme** fournie par l'enseignant.

Les modèles exécutés sur les portables des étudiants sont écartés : un modèle assez capable pour le mode agent (24B paramètres et plus) demande environ 24 Go de mémoire graphique, et les très petits modèles (Ministral, 3B à 14B) appellent mal les outils sur une tâche Symfony qui touche plusieurs fichiers.

## Fournisseur : décision après essai

Deux fournisseurs restent en lice. Tous deux facturent sur crédits prépayés et plafonnent la dépense **par workspace, pas par clé** : on crée un workspace par binôme, avec sa clé et son plafond mensuel.

| | Mistral (La Plateforme) | Anthropic (Claude Console) |
|---|---|---|
| Modèle envisagé | Devstral Small 2 (`devstral-small-2512`, 24B, 68 % sur SWE-bench Verified selon Mistral) | Claude Haiku 5.5, Claude Sonnet 5.5 |
| Prix par million de tokens (entrée / sortie) | 0,10 $ / 0,30 $ via Vercel ; aucun tarif officiel trouvé chez Mistral | Haiku 5.5 : 0,10 $ / 0,50 $ jusqu'à 100 000 tokens de prompt, 0,50 $ / 2,50 $ au-delà ; Sonnet 5.5 : 2 $ / 10 $, lecture du cache à 0,20 $ |
| Coût estimé d'une session d'agent | Quelques centimes | 0,10 à 0,30 $ (Haiku), environ 1 $ (Sonnet) |
| Plafond par workspace | Oui, réglé dans la console | Oui, réglé dans la console, avec alertes |
| Limites de débit | Par workspace, partagées entre ses clés | Par workspace, réglables par modèle ; le cache ne compte pas dans la limite d'entrée |
| Création des workspaces | À la main, nombre maximum non documenté | Scriptable (Admin API), 100 par organisation |
| Dans VS Code | Custom Endpoint (API Chat Completions) | Fournisseur Anthropic intégré |
| Autre | Fournisseur européen ; vérifier si les requêtes du mode gratuit servent à l'entraînement | Une nouvelle organisation démarre au niveau Evaluation, aux limites réduites : créer le compte bien avant mai 2027 |

L'essai consiste à lancer un prompt d'expérience sur les deux branches du fil rouge avec chaque modèle candidat, en relevant la consommation réelle. Le modèle retenu doit réussir au moins sur la branche `outille` ; un modèle moins capable peut rendre l'écart plus visible, mais s'il échoue partout, la séance 6 tombe à plat.

Si aucun fournisseur ne convient, les solutions de repli sont un serveur GPU loué le temps du cours (vLLM avec Devstral Small 2 ou Qwen3-Coder-30B-A3B, une carte de 80 Go pour une quinzaine de sessions simultanées), ou la démonstration en direct par l'enseignant.

**Conséquence** : les slides de la séance 6 montrent la configuration BYOK de VS Code, et les conditions de Copilot Student, des fournisseurs et de leurs modèles sont à revérifier en avril 2027.
