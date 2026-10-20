# AGENTS.md

Slides du cours **Développement PHP assisté par IA**, en français, servies par [revealexpress](https://github.com/pjehan/RevealExpress) (reveal.js + Express).

## Cadre du cours

- `README.md` : plan détaillé (séances, durées, objectifs, mise en pratique). Un chapitre rédigé couvre tout ce que son plan annonce ; un écart de plan se décide avec l'utilisateur et se reporte dans le README.
- `CONTEXT.md` : vocabulaire du cours. Emploie ses termes et évite ceux listés en _Avoid_ (ex. « capteur », jamais « sensor » ; « harnais », jamais « harness »).
- `docs/adr/` : décisions structurantes. Le harnais comme fil conducteur et les méthodes de spec-driven development présentées sans être appliquées (0001), le fil rouge fourni avec une dette volontaire (0002), l'expérience avec et sans harnais (0003), l'agent Copilot avec un modèle commun en BYOK (0004).
- `docs/preparation.md` : ce que l'enseignant doit préparer avant les séances (fil rouge, tests d'acceptation cachés, essai des modèles).
- Les séances 7 et 8 du module (MCP) ont leur propre slideshow, `../mcp` : ne pas y empiéter.

## Les outils évoluent vite : vérifie avant d'écrire

Agents, modèles, offres et outils PHP changent plusieurs fois par an ; tes connaissances sont probablement en retard. Avant de rédiger ou de modifier une slide, **confronte chaque affirmation à la documentation officielle** de l'outil, et cite la version utilisée quand une slide décrit son comportement. Termine la slide par un lien `<footer>` vers la page de documentation correspondante.

Informations vérifiées le **20/10/2026**, à revérifier en priorité avant les séances de mai 2027 :

- **Copilot Student** : 200 crédits IA par mois, sélection automatique du modèle uniquement, complétions illimitées ([plans](https://docs.github.com/en/copilot/get-started/plans), [facturation](https://docs.github.com/en/copilot/concepts/billing-and-usage/individuals/billing), [annonce #189268](https://github.com/orgs/community/discussions/189268)).
- **BYOK dans VS Code** : mode agent possible sans plan Copilot, pas de complétions par un modèle BYOK ([documentation](https://code.visualstudio.com/docs/agent-customization/language-models)).
- **Fournisseurs et modèles** de l'ADR 0004 : prix, plafonds par workspace, limites de débit ([Mistral](https://docs.mistral.ai/admin/workspaces/usage-limits), [Anthropic](https://platform.claude.com/docs/en/manage-claude/workspaces)).
- **Composer** : `config.policy` et blocage des malwares depuis la 2.10 (mai 2026) ; `minimum-release-age` pas encore livré (issue #12847) ([documentation](https://getcomposer.org/doc/), [article de Sebastian Bergmann](https://phpunit.expert/articles/composer-and-packagist-under-supply-chain-stress.html)).
- **Symfony AI Mate** : CLI sans serveur MCP depuis Symfony AI 0.13, instructions écrites dans `AGENTS.md` ([documentation](https://symfony.com/doc/current/ai/components/mate.html)).
- **Spec-driven development** : commandes de GitHub Spec Kit et d'OpenSpec, version de BMAD ([Spec Kit](https://github.com/github/spec-kit), [BMAD](https://github.com/bmad-code-org/BMAD-METHOD)).
- **Slopsquatting** : chiffres de Spracklen et al. (USENIX Security 2025) et de la réplication de 2026 ; aucun cas avéré sur Packagist à cette date.

Si une information a changé : mets à jour les slides, l'ADR ou le README concerné, puis cette liste et sa date. Signale à l'utilisateur tout contenu devenu obsolète.

## Lancer le slideshow

Ce slideshow est conçu pour **revealexpress 2.0**, en cours de développement dans `../../revealexpress` (la version publiée sur npm est plus ancienne) :

```bash
npm install
npm run build      # Vite compile src/ vers assets/ (ou npm run watch)
npm start          # revealexpress local sur http://<ip>:3000 (--port pour changer), sans ouvrir le navigateur
```

`npm start` exécute directement les sources TypeScript de `../../revealexpress` (Node.js ≥ 22.18). Le HTML est relu à chaque chargement de page ; seul `src/` demande un nouveau build.

En cas de doute sur le comportement du serveur ou du client, la référence est le code de `../../revealexpress/src/` (le README y est parfois en retard).

## Structure

- Chaque `NN-slug.html` à la racine est un **chapitre**, concaténé par ordre alphabétique : une `<section id="slug">` (id unique) contenant des `<section>` verticales. Première slide en `<h2>`, les suivantes en `<h3>`. `01-intro.html` est la page de titre.
- Build **Vite** (`vite.config.mjs`) en mode bibliothèque IIFE, car revealexpress charge les fichiers avec des balises `<script>` et `<link>` classiques déclarées dans `slideshow.config.js` : `src/main.js` → `assets/js/script.js`, styles → `assets/css/style.css`. `assets/` est entièrement régénéré à chaque build et ignoré par git.
- Images et fichiers statiques : dans `src/public/` (copié tel quel dans `assets/`). `src/public/images/logo.png` se référence `assets/images/logo.png` dans le HTML.
- `src/js/prism.js` : langages, plugins et thème Prism, importés explicitement (actuellement php, bash, json, yaml, markdown, sql, twig, diff). Tout nouveau langage de bloc de code s'y importe **après les langages dont il dépend** (champ `require` de `node_modules/prismjs/components.json`, ex. `markup-templating` avant `php`), puis `npm run build`.
- Slides en **1280×800** (`revealjs` dans `slideshow.config.js`) et blocs de code limités à 620 px de haut : découpe le code au-delà d'environ 25 lignes.
- **reveal.js 6** (thème solarized, `history: true`, `slideNumber: true`, options surchargeables dans la clé `revealjs` de `slideshow.config.js`) : fragments, `data-auto-animate`, `r-stretch`, `r-fit-text`, `r-stack` et les fonds `data-background-*` sont disponibles. Aucun plugin reveal.js n'est enregistré : pas de vue présentateur (les `<aside class="notes">` restent simplement masquées), pas de markdown ni de plugin highlight (Prism s'en charge).
- L'URL ne reprend que l'`id` de la **slide affichée** : une slide vers laquelle on veut pointer (`href="#/mon-id"`) porte son propre `id`, unique dans tout le slideshow.
- Script de slide : dans `src/main.js`, `event.detail.Reveal` de l'événement `loaded` est l'**instance** du slideshow (`deck.on('slidechanged', …)`, `deck.getCurrentSlide()`), pas l'objet global `Reveal`.

## Code dans les slides

- Tout le code du cours est en **PHP avec Symfony**, sur le fil rouge `helpdesk` (ADR 0002). Les configurations d'outils sont en PHP (`.php-cs-fixer.dist.php`, `rector.php`), en NEON (`phpstan.neon`, rendu en `language-yaml`) ou en YAML (`deptrac.yaml`, workflows GitHub Actions).
- `<pre><code class="language-php">` suivi immédiatement de la première ligne ; `</code></pre>` collé à la dernière.
- Échappe tout le code : `<` → `&lt;`, `>` → `&gt;`, `&` → `&amp;`.
- `AGENTS.md`, prompts et spécifications en `language-markdown` ; modifications proposées par un agent en `language-diff` ; sorties de commandes (`composer check`, PHPStan, Infection) en `language-none`.
- Diagrammes (boucle de l'agent, barrières successives, couches Deptrac) en **Mermaid**, jamais en art ASCII : `<pre class="mermaid">flowchart LR` suivi du code, `>` échappé. `src/js/mermaid.js` porte le thème solarized et dessine les diagrammes d'une slide à son affichage.
- Un seul langage d'implémentation : les extraits sont de simples `<pre><code>`, sans onglets de langage.

Avec Claude Code, le skill `revealexpress` détaille les conventions et fournit un script de vérification.
