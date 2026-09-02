---
name: beat-tom-design
description: Design system de Beat Tom — palette, composants canoniques, règles visuelles non négociables, patterns d'écran. À charger avant toute création ou modification d'interface. Se déclenche sur « refais le design », « applique le style Beat Tom », « stylise cet écran », « utilise la palette », « couleurs du projet », « thème sombre Beat Tom ».
---

# Beat Tom · Design system

Application anti-procrastination sans culpabilisation. Tom est la personnification de la procrastination, présentée comme un adversaire externe.

Règle directrice de toute décision visuelle : **Tom perd toujours, l'utilisateur ne perd jamais.**

## Palette

> **À confirmer depuis Figma.** Les valeurs ci-dessous sont des replis relevés sur une capture d'écran de la maquette, donc approximatifs. Les remplacer par les hex exacts dès qu'ils sont disponibles, dans ce fichier **et** dans `src/constants/theme.ts`.

| Rôle | Valeur | Usage |
|---|---|---|
| `bg` | `#12101E` | fond d'écran principal |
| `surface` | `#1E1B2E` | cartes projet, barre d'onglets, feuilles |
| `surface-raised` | `#282438` | états pressés, bordures de cartes |
| `accent` | `#E94FC7` | bouton d'action flottant, mot « tom », jour sélectionné |
| `accent-lime` | `#B8E64A` | traits de la mascotte, accents secondaires |
| `text-primary` | `#FFFFFF` | titres, valeurs de durée |
| `text-secondary` | `#A09CB8` | libellés, dates, texte d'appoint |

### Couleurs de pastilles projet

Cinq teintes distinguables sur fond sombre, proposées automatiquement à la création d'un projet : **magenta, vert lime, cyan, jaune, violet**.

Hex à confirmer depuis Figma. En attendant, réutiliser `accent` pour le magenta et `accent-lime` pour le vert lime ; les trois autres ne sont pas encore définies et ne doivent pas être inventées en dur dans un composant.

## Règles visuelles non négociables

Dérivées des principes de conception du produit. Une violation remonte à Liz avant implémentation.

1. Une journée sans activité s'affiche **vide et neutre**. Jamais de croix, jamais de rouge, jamais d'icône d'alerte.
2. Le rouge n'existe pas dans l'interface en dehors des erreurs techniques.
3. Aucun élément d'interface ne compare l'utilisateur à une cible, ni à lui-même dans le passé, de manière évaluative.
4. Un projet travaillé plusieurs fois dans la même journée produit **une seule pastille** sur le calendrier.
5. Les états vides portent un message bienveillant, jamais un constat de manque.
6. Aucune confirmation demandée sur les actions non destructives.

## Composants canoniques

| Composant | Composition |
|---|---|
| Carte projet | nom, pastille de couleur, durée cumulée, mention temporelle secondaire |
| Cellule de jour du calendrier | numéro, rangée de pastilles, contour arrondi pour le jour sélectionné, point discret pour le jour courant |
| Bouton d'action flottant | magenta (`accent`), action principale d'ajout |
| Barre d'onglets | trois entrées, fond `surface` |
| En-tête d'accueil | avatar de Tom, message bienveillant variable |

## Patterns d'écran

- Fond sombre uniforme (`bg`), sans dégradé de section.
- Contenu en colonne unique.
- Toute saisie passe par une feuille inférieure (`@gorhom/bottom-sheet`), jamais par un écran plein dédié.
- Les projets actifs s'affichent en liste horizontale défilante.

## Contrainte technique

Le styling passe par **NativeWind v4**. La palette est déclarée en tokens dans `beat-tom-mobile/src/constants/theme.ts` et exposée à Tailwind.

**Aucune couleur en dur dans un composant.** Une couleur littérale (`#E94FC7`, `rgb(...)`, `bg-[#...]`) dans un fichier de composant est un défaut à corriger, pas un raccourci acceptable.

Les composants d'interface viennent de gluestack UI v3, copiés dans `src/components/ui/` et modifiés sur place — ce ne sont pas des dépendances, ce sont des fichiers du projet.
