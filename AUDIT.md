# AUDIT.md

## 1. Audits outillés

Audits lancés sur la page de départ (avant correction), en local avec `npm run dev`.

- **Lighthouse** (DevTools → onglet Lighthouse → catégorie Accessibility) : score **86/100**. Un seul audit en échec sur le contraste (`color-contrast`, "Background and foreground colors do not have a sufficient contrast ratio") avec 5 éléments remontés : `p.subtitle`, les deux `p.label`, `p.hint` et `button.btn-primary`. Lighthouse donne pour chacun le ratio mesuré et les couleurs (voir tableau). Il remonte aussi un point hors contraste : `landmark-one-main`, la page n'a pas de balise `<main>`. Tout le reste passe (titres, boutons nommés, langue du document, etc.).
- **axe DevTools** (extension Chrome) : 2 règles en échec. `color-contrast` (impact *serious*) avec les mêmes 5 éléments que Lighthouse, c'est normal puisque Lighthouse utilise axe-core en interne. Et `landmark-one-main` / `region` (impact *moderate*) parce que le contenu n'est pas dans un landmark. Aucune erreur *critical*.
- **WAVE** (https://wave.webaim.org/, ou l'extension) : 0 erreur de structure (le `h1` est là, les boutons ont un texte, la langue est définie) mais **5 erreurs de contraste** "Very low contrast" sur exactement les mêmes textes gris et sur le bouton bleu. WAVE affiche aussi une alerte sur l'absence de région `main`.

Les 3 outils sont d'accord : le seul vrai problème de la page, ce sont les couleurs.

## 2. Les éléments à corriger

| Élément | Couleur texte | Couleur fond | Ratio mesuré | Ratio AA requis | Conforme ? |
|---------|---------------|--------------|--------------|------------------|------------|
| `.subtitle` | `#aaaaaa` | `#ffffff` | 2.32:1 | 4.5:1 | Non |
| `.label` | `#999999` | `#ffffff` | 2.84:1 | 4.5:1 | Non |
| `.hint` | `#bbbbbb` | `#ffffff` | 1.91:1 | 4.5:1 | Non |
| `.btn-primary` | `#ffffff` | `#6fa8dc` | 2.52:1 | 4.5:1 (texte) | Non |

Les 4 éléments sont du texte normal (16px ou moins, pas en gras), donc c'est bien le seuil 4.5:1 qui s'applique et pas 3:1.

### Après correction (`src/App.css`)

| Élément | Couleur texte | Couleur fond | Nouveau ratio | Conforme AA ? |
|---------|---------------|--------------|---------------|---------------|
| `.subtitle` | `#6b6b6b` | `#ffffff` | 5.33:1 | Oui |
| `.label` | `#666666` | `#ffffff` | 5.74:1 | Oui |
| `.hint` | `#666666` | `#ffffff` | 5.74:1 | Oui |
| `.btn-primary` | `#ffffff` | `#2b6cb0` | 5.42:1 | Oui |

J'ai gardé l'idée du design : des gris doux pour les textes secondaires et un bouton bleu, juste un peu plus foncés. Lighthouse relancé après correction : **96/100** sur l'accessibilité, plus aucune erreur de contraste. Il ne reste que le point sur la balise `<main>` manquante.

## 3. Priorisation

1. **`.btn-primary`** : c'est l'action principale de la page ("Gérer mon abonnement"). Si une personne malvoyante ne lit pas le texte du bouton, elle ne peut pas faire ce pour quoi elle est venue. C'est le seul élément interactif concerné, donc le plus urgent.
2. **`.hint`** : c'est le pire ratio (1.91:1, presque illisible même pour quelqu'un sans trouble de la vue en plein soleil). Le texte donne une info utile sur la facturation et le renouvellement automatique, la rater peut coûter de l'argent à l'utilisateur.
3. **`.label`** : les labels donnent le sens des valeurs ("Statut de l'abonnement", "Astuce"). Sans eux, on voit une date sans savoir à quoi elle correspond. En plus le texte est petit (13.6px), ce qui aggrave le problème. Il apparaît deux fois sur la page.
4. **`.subtitle`** : ratio 2.32:1, à corriger aussi, mais c'est une phrase d'accroche qui ne porte pas d'information indispensable. L'utilisateur peut quand même utiliser la page s'il ne la lit pas, donc c'est le moins urgent des 4.

En bonus, ajouter une balise `<main>` autour du contenu ferait passer le dernier point remonté par les outils, mais ce n'est pas un problème de contraste.
