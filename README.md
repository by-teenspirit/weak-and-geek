# weak and geek — feuille de style

Habillage du forum Forumactif **a dream within a dream** (thème ModernBB).

## Fichier

| Fichier | Rôle |
|---|---|
| `adwad.css` | Toute la feuille de style du forum |

## Comment le forum charge ce fichier

Le template **`overall_header`** contient, juste avant `</head>` :

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/by-teenspirit/weak-and-geek@main/adwad.css?v=2" />
```

Le `?v=2` n'est pas décoratif : jsDelivr renvoie
`Cache-Control: max-age=604800`, donc **le navigateur garde le fichier
7 jours**. Sans ce numéro, une modification ne serait visible par personne
avant une semaine.

Le CSS de l'administration (Affichage ▸ Couleurs ▸ Feuille de style CSS) est
**vide volontairement** : tout se modifie ici, dans ce dépôt.

Raison : Forumactif coupe silencieusement une feuille de style au-delà
d'environ **65 000 caractères**. Le fichier en fait plus de 70 000.

## Modifier le style

1. Éditer `adwad.css` dans ce dépôt et committer.
2. Vider le cache de jsDelivr, sinon la modification met jusqu'à 12 h à sortir :

   <https://purge.jsdelivr.net/gh/by-teenspirit/weak-and-geek@main/adwad.css>

3. **Incrémenter le `?v=` dans `overall_header`** (`?v=2` → `?v=3`),
   enregistrer le template **et le publier**. C'est cette étape qui fait
   sortir la modification chez les visiteurs ; sans elle, leur navigateur
   continue de servir l'ancienne version pendant 7 jours.
4. Recharger le forum (Ctrl/Cmd + Maj + R).

## Points de vigilance

- **L'ordre compte.** Le fichier est chargé *après* la feuille de base de
  ModernBB : il la surcharge. Ne pas déplacer le `<link>` plus haut dans
  `overall_header`.
- **Le plugin Switcheroo** charge sa propre feuille depuis le template
  `overall_footer_end`, donc *après* celle-ci. Les variables de thème du
  plugin sont donc redéfinies sur `html:root` (spécificité 0-1-1), qui
  l'emporte sur son `:root` (0-1-0) quel que soit l'ordre. Ne pas
  « simplifier » ces sélecteurs en `:root`.
- **Points de rupture** utilisés : 1100px, 950px, 700px, 420px.
- La feuille est minifiée. C'est le seul état vérifié comme fonctionnel :
  une tentative de dé-minification l'avait rendue inexploitable.
