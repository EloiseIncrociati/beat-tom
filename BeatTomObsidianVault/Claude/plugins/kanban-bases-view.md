---
type: reference
tags: [claude, internal, plugins]
plugin-id: kanban-bases-view
status: actif
---

# Kanban Bases View

**id** `kanban-bases-view` · **Chemin** `.obsidian/plugins/kanban-bases-view/` · **Données** `Beat Tom Board.base` à la racine du vault · rend une vue `kanban-view` à l'intérieur d'un fichier Bases, colonnes et couloirs pilotés par le frontmatter des notes.

## Axes / Config

Ce plugin ne stocke rien d'utile dans son `data.json`. Toute la configuration vit dans les vues `type: kanban-view` du fichier `.base`.

| Clé | Rôle | Valeur Beat Tom |
|---|---|---|
| `groupByProperty` | propriété qui définit les colonnes | `note.status` (vues Status, Bugs), `note.priority` (vue Priorité) |
| `swimlaneProperty` | propriété qui définit les couloirs horizontaux | `note.lot` (vue Status), `note.priority` (vue Bugs) |
| `quickAddFolder` | dossier où atterrit une carte créée depuis le kanban | `Board` |
| `columnOrders` | ordre explicite des colonnes | `[Roadmap, To Do, Doing, Done]` |
| `columnColors` | couleur par valeur de colonne | `Doing: yellow`, `Done: green`, `Uncategorized: red` |
| `cardOrders` | ordre manuel des cartes dans chaque colonne | **laisser vide**, le plugin le remplit lui-même |
| `wrapPropertyValues` | retour à la ligne des valeurs affichées sur la carte | `false` sur la vue Status |
| `order` | propriétés affichées sur la carte | `[id, platform, lot, priority]` |

Le regroupement en couloirs par `lot` sur la vue Status est délibéré : la roadmap est structurée en lots séquentiels, et voir un lot 2 démarrer alors que le lot 1 n'est pas terminé est exactement le signal qu'on veut rendre visible.

## Frontmatter attendu

```yaml
---
id: F-006
platform: mobile
type: feature
lot: 1
status: To Do
priority: Medium
date: 2026-09-01
assignee: Claude Code
---
```

Valeurs autorisées : voir le tableau du `CLAUDE.md` du vault.

## Opérations

| Action | Comment |
|---|---|
| Créer une carte | écrire `Board/<titre>.md` avec le frontmatter complet. Elle apparaît dans le kanban au reload. |
| Déplacer une carte de colonne | éditer `status` dans le frontmatter de la note, pas le `.base` |
| Changer une carte de couloir | éditer `lot` (vue Status) ou `priority` (vue Bugs) |
| Réordonner les colonnes | éditer `columnOrders` dans le `.base` — **règle de course applicable** |
| Ajouter une vue | ajouter une entrée sous `views:` dans le `.base` — **règle de course applicable** |
| Retirer une carte du board | passer `status` à `Done`, ou déplacer le fichier hors de `Board/` (le filtre `file.inFolder("Board")` la fait disparaître) |

**Règle de course** : Obsidian réécrit `.base` depuis sa mémoire à chaud. Fermer l'onglet du board, vérifier que le hash du fichier est stable, puis écrire. Les `.md` de `Board/` sont éditables à tout moment.

Voir aussi [[core-bases]], [[core-templates]]
