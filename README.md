# weak and geek — feuille de style

Habillage du forum Forumactif **a dream within a dream** (thème ModernBB).

| Fichier | Rôle |
|---|---|
| `adwad.css` | Toute la feuille de style du forum |

## Comment le forum charge ce fichier

Le template **`overall_header`** contient, juste avant `</head>` :

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/by-teenspirit/weak-and-geek@<SHA>/adwad.css" />
```

`<SHA>` est le **hash du commit**, pas `@main`. C'est volontaire :

- une URL figée à un commit est **immuable**, donc jsDelivr la sert
  immédiatement, sans purge et sans attente ;
- `@main` est renvoyé avec `Cache-Control: max-age=604800` : le navigateur
  des visiteurs garde le fichier **7 jours**, et le purge jsDelivr est
  **limité à un appel toutes les 30 minutes environ**.

Le CSS de l'administration (Affichage ▸ Couleurs ▸ Feuille de style CSS) est
**vide volontairement** : tout se modifie ici. Raison : Forumactif tronque
silencieusement une feuille au-delà d'environ 65 000 caractères, et celle-ci
en fait plus de 83 000.

## Publier une modification

1. Éditer `adwad.css` ici et committer.
2. Copier le hash du nouveau commit (page *Commits*).
3. Dans `overall_header`, remplacer l'ancien hash par le nouveau,
   **enregistrer puis publier** le template.
4. Recharger le forum.

L'étape 3 n'est pas facultative : sans changement d'URL, rien ne sort.

> **Piège à éviter :** ne jamais reconstruire le fichier à partir de
> `@main` sur le CDN — il peut être périmé de plusieurs commits et on
> écrase alors du travail. Repartir du dépôt, ou d'une URL figée au commit.

## Points de vigilance

- **L'ordre compte.** Le fichier est chargé *après* la feuille de base de
  ModernBB : il la surcharge. Ne pas remonter le `<link>` dans
  `overall_header`.
- **Switcheroo** charge sa propre feuille depuis `overall_footer_end`, donc
  *après* celle-ci. Ses variables sont redéfinies sur `html:root`
  (spécificité 0-1-1), qui bat son `:root` (0-1-0) quel que soit l'ordre.
  Ne pas « simplifier » ces sélecteurs en `:root`.
- **`#main-content` est une colonne flex.** L'espacement vertical se règle
  par son `gap` (`--adwad-gap`, 24px ; 40px sur l'accueil), jamais par des
  marges sur les enfants — elles s'ajoutent au gap et cassent le rythme.
- **Rayons** : `--adwad-radius-xs` 3px, `sm` 5px, `md` 8px, `lg` 20px
  (la carte principale). Aucun cercle, aucune valeur en dur.
- **Points de rupture** : 1100px, 950px, 700px, 480px, 420px.
- La feuille est minifiée. C'est le seul état vérifié comme fonctionnel.

## Vérifier avant de committer

```bash
npm install css-tree
node -e "
const c=require('css-tree'), fs=require('fs');
let err=[]; const ast=c.parse(fs.readFileSync('adwad.css','utf8'),{positions:true,onParseError(e){err.push(e.message)}});
let r=0; c.walk(ast,n=>{if(n.type==='Rule')r++});
console.log('règles:',r,'| erreurs:',err.length); err.forEach(e=>console.log(' !',e));
"
```

État de référence : **676 règles, 21 `@media`, 0 erreur**.
