# UxNote

[English](README.md) | [Français](README.fr.md)

<p align="center">
  <a href="https://uxnote.ninefortyone.studio">
    <img alt="Docs et installation personnalisée disponible sur la landing page" src="assets/badges/landing-page-fr.svg" height="40">
  </a>
  <a href="https://www.buymeacoffee.com/ninefortyonestudio">
    <img alt="Buy me a coffee" src="assets/badges/bmc-button%20yallow.svg" height="40">
  </a>
</p>

Uxnote est une barre d'annotation pour maquettes et sites web. Ajoutez un seul script pour obtenir des surlignages, des marqueurs d'elements, des cartes numerotees, des couleurs personnalisees, un mode focus assombri, l'import/export et un envoi par email. Sans plugin et sans backend.

## Pour qui
- Agences et freelances : les clients commentent directement sur la page, puis vous exportez un fichier de revue propre.
- Equipes produit et UX : validez dans le navigateur la ou l'interface vit, sans toucher au code existant.

## Fonctions principales
- Surlignages de texte et epingles d'elements avec pastilles numerotees.
- Annotation d'elements possible dans les modales et dialogues. Utilisez `↑` / `↓` pour choisir le bon calque lorsque plusieurs elements se superposent.
- Nom du relecteur facultatif : c'est le commentaire qui compte.
- Couleurs unifiees ou par type. Le voile assombri est desactive par defaut et peut etre active.
- Import et export dans un fichier JSON unique (titre + date), avec re-import.
- **Export pour IA** : un brief Markdown qui indique a un assistant de code ou se trouve chaque note et quoi modifier (voir ci-dessous).
- Envoi par email pour partager les retours avec les developpeurs.

## Raccourcis clavier
- `Alt+V` (`Option+V` sur macOS) : afficher ou masquer toute la couche Uxnote.
- `↑` / `↓` en mode element : passer d'un element superpose a l'autre sous le pointeur (par exemple le fond d'une modale et le contenu derriere).

Le panneau des notes est masque a chaque chargement de page. Ouvrez-le avec le bouton panneau de la barre d'outils.

## Export pour IA
L'export IA ecrit un fichier Markdown (`*-ai.md`) et le copie dans le presse-papiers. Il est disponible a trois endroits :
- le bouton **etincelle** de la barre d'outils (toutes les notes) ;
- le bouton **Export pour IA** de la fenetre d'export (filtree par relecteur et priorite) ;
- le bouton de copie sur chaque carte de note (une seule note).

Chaque note donne l'URL de la page, le texte de l'element, un selecteur CSS, un XPath, le chemin des ancetres, les attributs stables (`id`, `data-testid`, `aria-label`, ...), le titre le plus proche, la modale englobante si elle existe, et le commentaire. Le fichier indique aussi a l'assistant comment localiser l'element, evaluer le commentaire, appliquer le plus petit changement et rendre compte.

## Comment ca fonctionne
1. Injectez le script sur chaque page (ou via un tag manager global).
2. Partagez l'URL avec votre client.
3. Les clients annotent le texte ou les elements ; tout apparait dans le panneau Uxnote.
4. Exportez le JSON ou envoyez par email pour collecter et traiter les retours.

## Installation (copier/coller)
Placez le script juste avant `</body>` pour que le DOM soit pret. Si vous devez le mettre dans `<head>`, ajoutez `defer`.

```html
<script src="https://cdn.jsdelivr.net/gh/marouane-tabib/uxnote@main/dist/uxnote.min.js"></script>
```

Le lien est servi par [jsDelivr](https://www.jsdelivr.com/) depuis le fichier `dist/uxnote.min.js` de la branche `main` de `marouane-tabib/uxnote`. Aucune release ni version n'est necessaire. Le fichier se met a jour quand vous poussez un nouveau build sur `main`.

## Build
Le script est construit depuis `uxnote-tool/uxnote.js` avec esbuild.

Prerequis : Node.js 18 ou plus recent.

```bash
npm install      # une seule fois, installe esbuild
npm run build    # ecrit dist/uxnote.min.js et dist/uxnote.min.js.map
```

Pour publier un nouveau build :
1. Lancez `npm run build`.
2. Committez `uxnote-tool/uxnote.js` et `dist/uxnote.min.js` (et le `.map`), puis poussez sur `main`.
3. Le lien ci-dessus sert le nouveau fichier. jsDelivr peut garder une copie pendant quelques heures ; pour la rafraichir tout de suite, ouvrez `https://purge.jsdelivr.net/gh/marouane-tabib/uxnote@main/dist/uxnote.min.js`.

Pour utiliser le fichier dans un projet sans CDN, copiez `dist/uxnote.min.js` dans votre projet et referencez-le localement, par exemple `<script src="/js/uxnote.min.js"></script>`.

## Options de la balise script
Le builder de la landing expose ces options :
- `colorForHighlight` ou `colorForTextHighlight` + `colorForElementHighlight`
- `isBackdropVisible` (desactive par defaut ; mettre `"true"` pour assombrir la page derriere la barre)
- `isToolOnTopAtLaunch`
- `isToolVisibleAtFirstLaunch`
- `data-mailto` (destinataire pour l'export email)

Vous pouvez aussi bloquer des zones avec `data-uxnote-ignore`, et re-activer un enfant avec `data-uxnote-allow`.

## Stockage et donnees
Les annotations sont stockees dans `localStorage` pour l'origine courante et par URL. Aucune donnee n'est envoyee a un serveur sauf si vous exportez un fichier JSON ou envoyez des annotations par email.

## Notes de compatibilite
- Fonctionne sur staging, previews ou localhost tant que le script se charge et que `localStorage` est autorise.
- Pour les SPA, les changements de route peuvent demander un rechargement ou une re-init pour afficher les annotations de la nouvelle URL.
- Si la CSP est stricte, autorisez l'origine du script Uxnote et les styles inline (ou ajoutez un nonce/hash).
- Les iframes same-origin fonctionnent si vous injectez Uxnote dans le document de l'iframe.

## Licence
Uxnote est publie sous licence MIT. Voir `LICENSE`.

## Structure du projet
- `index.html` - landing page et texte de documentation.
- `assets/` - styles de la landing et donnees de langue.
- `uxnote-tool/uxnote.js` - script Uxnote (source).
- `scripts/build.js` - etape esbuild qui produit le fichier minifie dans `dist/`.
- `dist/uxnote.min.js` - script construit, servi par le lien d'installation.
- `CHANGELOG.md` - historique des versions.
