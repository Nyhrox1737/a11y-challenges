# AUDIT.md

Pour chaque ligne, retrouve le problème correspondant dans `src/App.tsx` / `src/App.css`
et remplis les 3 colonnes. Il y a **6 problèmes volontaires** dans ce starter.

| # | Problème trouvé (où, quoi) | Principe WCAG (P / O / U / R) | Pourquoi c'est bloquant, et pour qui |
|---|------------------------------|-------------------------------|----------------------------------------|
| 1 | `App.tsx` ligne 19 : `<img src="/hero.jpg" />` n'a pas d'attribut `alt` | P | Un lecteur d'écran ne peut rien dire de l'image, il lit juste le nom du fichier. Une personne aveugle ou malvoyante perd l'info. Si c'est une image décorative il faudrait au moins `alt=""`. |
| 2 | `App.tsx` ligne 21 : on passe directement du `h1` à un `h3`, il n'y a pas de `h2` | R | Les utilisateurs de lecteur d'écran naviguent de titre en titre. Un saut de niveau casse le plan de la page, on ne sait plus si on a raté une section. Ça pénalise aussi le SEO. |
| 3 | `App.tsx` ligne 37 : "Voir plus" est un `<div className="see-more" onClick>` et pas un `<button>` | O | Un `div` n'est pas focusable avec Tab et ne réagit pas à Entrée/Espace, donc impossible à utiliser au clavier. Le lecteur d'écran l'annonce comme du texte normal, l'utilisateur ne sait même pas qu'on peut cliquer dessus. |
| 4 | `App.tsx` ligne 33 : le bandeau `.carousel` newsletter s'affiche sans bouton pour le fermer ou le mettre en pause | O | Critère WCAG 2.2.2 : un contenu qui apparaît ou bouge doit pouvoir être contrôlé par l'utilisateur. Ça gêne les personnes avec des troubles de l'attention ou cognitifs et celles qui utilisent un lecteur d'écran (le bandeau coupe la lecture). |
| 5 | `App.tsx` lignes 42 à 44 : les 3 liens ont tous le même texte "cliquez ici" | U | Beaucoup d'utilisateurs de lecteur d'écran affichent la liste des liens de la page hors contexte. Avec trois fois "cliquez ici" on ne sait pas où mène chaque lien. Le texte du lien doit décrire sa destination. |
| 6 | `App.tsx` ligne 26 + `App.css` `.dot-red` / `.dot-green` : le statut "annulé" ou "publié" est donné uniquement par la couleur du rond | P | L'information ne doit jamais reposer que sur la couleur. Une personne daltonienne, un écran en noir et blanc ou un lecteur d'écran ne voient pas la différence. Il faut un texte ou une icône en plus. |

> Rappel : P = Perceptible, O = Opérable, U = Utilisable (Compréhensible), R = Robuste.
