# Protocole de test : Résumer avec l'IA

But : constater, à une date donnée, ce que fait chaque assistant quand on clique sur son bouton, puis refaire exactement le même test plus tard (t2) pour voir ce qui a changé.

## Avant le test

1. **La page doit être en ligne à son adresse canonique** (elle peut rester en noindex). Le prompt contient l'URL canonique (`https://no-mad.io/labo/bouton-resumer-avec-ia?utm_source=resumer-ia&utm_medium=ai-button`). Une prévisualisation `*.pages.dev` ou un serveur local ne suffisent pas : les robots iraient lire une adresse qui n'existe pas encore.
2. **robots.txt servi** : `curl https://no-mad.io/robots.txt`. `ChatGPT-User`, `Claude-User` et `Perplexity-User` ne doivent pas être bloqués. Signaler tout bloc `# BEGIN Cloudflare Managed content`.
3. **Cloudflare** : vérifier les réglages **AI Crawl Control** (robots autorisés ou bloqués) et **Bot Preference Sync**. Noter l'état ici :
   - AI Crawl Control : …
   - Bot Preference Sync : …
4. **Vérifier la config** (`src/data/resumer-ia.yaml`, ou `CONFIG` dans `resumer-ia.html`) : toutes les adresses à tester sont actives et datées.
5. Noter le navigateur et sa version, l'appareil (ordinateur ou mobile) et l'offre de chaque assistant (gratuite ou payante).

## Déroulé, pour chaque assistant

1. Ouvrir la page dans le navigateur choisi. Noter si l'on est **connecté** à l'assistant. Les boutons sont sous le sommaire (colonne de droite, nom au survol) sur grand écran, et sous la barre du sommaire sur mobile.
2. Ouvrir « Voir le prompt envoyé » et vérifier qu'il est identique au prompt prévu.
3. Noter l'**heure exacte** du clic, puis cliquer sur le bouton.
4. Dans l'onglet de l'assistant, noter :
   - **Pré-rempli** : le prompt apparaît-il dans le champ ? (en mode clipboard : le message « Prompt copié » s'est-il affiché, et le collage fonctionne-t-il ?)
   - **Envoyé automatiquement** : la réponse démarre-t-elle sans action ?
   - **Avertissement affiché** : une fenêtre ou un message demande-t-il confirmation ? Recopier le texte.
5. Envoyer le prompt si besoin, attendre la réponse complète.
6. **Résumé fidèle** : 5 points, ce qu'il faut retenir, les limites, sans information absente de la page. Noter les erreurs.
7. **Lien vers la source présent** : la réponse contient-elle l'URL de la page ?
8. **Visite du robot** : dans Cloudflare (AI Crawl Control, ou Security > Events / Analytics filtré sur le chemin de la page), chercher une requête de `ChatGPT-User`, `Claude-User`, `Perplexity-User`, `MistralAI-User`… après l'heure du clic. Noter oui ou non, le robot, le statut HTTP et le délai.
9. Faire une capture d'écran de l'assistant et l'enregistrer avec la date et le nom de l'assistant.

Conseil : attendre quelques minutes entre deux assistants pour attribuer chaque visite de robot sans ambiguïté.

## Relevé à t

Date : …  ·  Navigateur : …  ·  Appareil : …

| Assistant | Date | Navigateur | Connecté (oui/non) | Pré-rempli | Envoyé automatiquement | Avertissement affiché | Visite du robot dans Cloudflare (oui/non, délai) | Résumé fidèle | Lien vers la source présent |
|---|---|---|---|---|---|---|---|---|---|
| ChatGPT | | | | | | | | | |
| Claude | | | | | | | | | |
| Perplexity | | | | | | | | | |
| Grok | | | | | | | | | |
| Copilot | | | | | | | | | |
| Le Chat | | | | | | | | | |
| Gemini (clipboard) | | | | | | | | | |

Remarques : …

## Relevé à t2

Date : …  ·  Navigateur : …  ·  Appareil : …

| Assistant | Date | Navigateur | Connecté (oui/non) | Pré-rempli | Envoyé automatiquement | Avertissement affiché | Visite du robot dans Cloudflare (oui/non, délai) | Résumé fidèle | Lien vers la source présent |
|---|---|---|---|---|---|---|---|---|---|
| ChatGPT | | | | | | | | | |
| Claude | | | | | | | | | |
| Perplexity | | | | | | | | | |
| Grok | | | | | | | | | |
| Copilot | | | | | | | | | |
| Le Chat | | | | | | | | | |
| Gemini (clipboard) | | | | | | | | | |

Changements depuis t : …

## Pour mémoire : vérification préalable des adresses (2026-11-17)

Navigateur intégré à Claude (Chromium), **non connecté**, prompt « test » sans l'adresse du site (pour ne pas fausser le relevé Cloudflare). Ce n'est pas le test : il sert seulement à vérifier que les adresses répondent.

| Assistant | Constat, déconnecté |
|---|---|
| ChatGPT | Paramètre `q` retiré de l'adresse, champ vide |
| Claude | Redirection vers la connexion, `q` perdu dans l'adresse de retour |
| Perplexity | Prompt envoyé automatiquement, réponse affichée |
| Grok | Prompt pré-rempli, fenêtre « Envoyer ce message ? » avant l'envoi |
| Copilot | Redirection vers copilot.com/fr-fr (connexion), `q` perdu |
| Le Chat | Redirection vers la connexion Mistral |
| Gemini | `q` ignoré, champ vide (d'où le mode clipboard) |
