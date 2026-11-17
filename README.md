# Résumer avec l'IA

Une rangée de boutons qui ouvrent un assistant IA (ChatGPT, Claude, Perplexity, Grok, Copilot, Le Chat, Gemini) avec un prompt déjà écrit, contenant l'URL de la page. Un seul fichier HTML + JS, sans dépendance, sans cookie, sans script tiers.

> C'est un constat à un instant t. Les assistants changent leurs adresses et leurs règles sans prévenir : chaque entrée porte une date `verifie`. Revérifiez-les avant d'utiliser le composant.

## Installation

1. Ouvrez `resumer-ia.html` dans un navigateur pour voir la démo.
2. Copiez le bloc compris entre `Résumer avec l'IA : copier d'ici...` et `...à ici` : un `<style>`, un `<div class="resumer-ia" data-lang="fr">` et un `<script>`.
3. Collez-le là où le composant doit apparaître. Ni tout en haut (il ferait partir le lecteur avant qu'il ait lu), ni seulement à la fin (il arriverait trop tard pour celui qui hésite devant un texte long). Un bon compromis : sous le sommaire, ou juste après l'introduction.
   - **WordPress** : bloc « HTML personnalisé » (le `<style>` et le `<script>` peuvent aller dans le thème ou dans un plugin d'extraits de code).
   - **Drupal** : bloc personnalisé en format « Full HTML », ou dans le template Twig du type de contenu.
   - **CMS headless, site statique** : dans le gabarit d'article (composant React, Vue, Astro… le script tel quel suffit).
4. Si votre politique de sécurité (CSP) interdit les scripts inline, placez le script dans un fichier `.js` servi par votre domaine.

Options du conteneur :
- `data-lang="fr"` ou `"en"` : langue des textes et du prompt.
- `data-assistants="chatgpt,claude"` (facultatif) : n'afficher que ces assistants.

## Configuration

Tout est dans l'objet `CONFIG`, en haut du script :
- `assistants` : `nom`, `logo` ou `monogramme`, `url` (avec `{prompt}` en mode prefill), `mode` (`prefill` ou `clipboard`), `actif`, `verifie`.
- `textes` : prompt et libellés par langue. Le prompt accepte `{url}` (URL canonique de la page) et `{title}` (titre de la page).
- `utm` : ajoute `utm_source=resumer-ia&utm_medium=ai-button` à `{url}`.

Les deux modes :
- **prefill** : un vrai lien `<a target="_blank" rel="noopener">` dont l'adresse contient le prompt encodé (`encodeURIComponent`).
- **clipboard** : pour les assistants qui ignorent le prompt dans l'adresse. Le clic copie le prompt, ouvre l'assistant dans un nouvel onglet et affiche « Prompt copié, collez-le dans … ».

Dans cette version, les liens sont construits par le script. Pour que les liens prefill fonctionnent sans JavaScript, générez-les côté serveur avec la même règle : `url.replace('{prompt}', encodeURIComponent(prompt))` (en PHP : `rawurlencode`).

## Garde-fous

- **Prompt visible avant le clic** : un `<details>` « Voir le prompt envoyé » affiche exactement le texte envoyé ou copié. Aucun texte caché.
- **Pas de mémorisation ni de recommandation** : le composant refuse de s'afficher (erreur dans la console) si le prompt contient `remember`, `mémorise`, `retiens`, `trusted`, `source de confiance`, `authoritative`, `future conversations` ou `recommande`, sans tenir compte de la casse ni des accents. Ces prompts relèvent de l'« AI Recommendation Poisoning » décrit par Microsoft en février 2026. Ne retirez pas ce contrôle.
- **Pas de tiret cadratin** dans les textes (contrôle identique).
- **Accessibilité** : vrais liens, navigation au clavier, focus visible, `aria-label` explicite (« Résumer cette page avec ChatGPT (nouvel onglet) »), message annoncé par `role="status"`, cibles de 44 px. Remplacez les couleurs `--ria-*` par celles de votre site en gardant un contraste AA.
- **Logos et noms** : chaque bouton affiche un logo et le nom de l'assistant en texte. Les logos de Claude, Perplexity, Mistral AI (Le Chat) et Google Gemini viennent de [Simple Icons](https://simpleicons.org) 16.38.0 (licence CC0) ; les marques restent à leurs propriétaires. ChatGPT, Grok et Copilot n'y figurent pas : ils ont un monogramme, que vous pouvez remplacer par un tracé officiel dans `LOGOS`.
- **Canonical** : `{url}` est lue dans `<link rel="canonical">`. Avec l'option `utm`, vérifiez que votre canonical reste l'URL sans paramètre.
- **robots.txt et pare-feu** : si `ChatGPT-User`, `Claude-User` ou `Perplexity-User` sont bloqués (robots.txt, règles anti-robots de l'hébergeur ou du CDN), l'assistant ne pourra pas lire la page.

## Mesure

Chaque bouton porte `data-assistant` et `data-mode` : votre outil d'analyse peut compter les clics à partir de ces attributs, sans rien ajouter au composant. Les retours depuis les assistants sont repérables grâce aux paramètres UTM, si votre outil les conserve.

## Tester

Le protocole et la grille de relevé sont dans [TEST.md](TEST.md).
