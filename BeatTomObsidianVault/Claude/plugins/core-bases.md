---
type: reference
tags: [claude, internal, plugins]
plugin-id: bases
status: actif
---

# Bases (core)

**id** `bases` · **Chemin** plugin core, activé dans `.obsidian/core-plugins.json` · **Données** les fichiers `.base` du vault, ici `Beat Tom Board.base` · moteur de base de données d'Obsidian : filtre les notes par frontmatter et les rend dans des vues (table, cards, et kanban via [[kanban-bases-view]]).

## Axes / Config

Un fichier `.base` est du YAML. Structure :

| Bloc | Rôle |
|---|---|
| `properties` | renommage d'affichage des propriétés de frontmatter (`note.<prop>` → `displayName`) |
| `views` | liste des vues. Chaque vue a un `type`, un `name`, des `filters` |
| `filters` | expressions combinées par `and` / `or`. Ex. `file.inFolder("Board")`, `note.type == "feature"` |
| `order` | colonnes ou propriétés affichées, dans l'ordre |
| `sort` | liste de `{ property, direction }` |

Types de vues utilisés dans Beat Tom : `kanban-view` (fourni par le plugin communautaire) et `table` (core).

Vues du board :

| Vue | Type | Filtre | Regroupement |
|---|---|---|---|
| Status | kanban-view | `Board` + `type == "feature"` | colonnes `status`, couloirs `lot` |
| Bugs | kanban-view | `Board` + `type == "bug"` | colonnes `status`, couloirs `priority` |
| Vue | table | `Board` | — |
| Priorité | kanban-view | `Board` | colonnes `priority` |

## Frontmatter attendu

Bases lit le frontmatter des notes, il n'en impose aucun. Les propriétés référencées par le board sont `id`, `platform`, `type`, `lot`, `status`, `priority`, `date`, `assignee`.

Une propriété absente du frontmatter fait tomber la note dans la colonne `Uncategorized` (colorée en rouge dans la vue Status, c'est un signal d'anomalie de saisie, pas un état métier).

## Opérations

| Action | Comment |
|---|---|
| Ajouter une propriété au board | l'ajouter dans `properties:` avec son `displayName`, puis dans l'`order` de la vue concernée |
| Filtrer sur une nouvelle valeur | ajouter une expression sous `filters.and` de la vue |
| Créer une nouvelle vue | ajouter une entrée sous `views:` avec `type`, `name`, `filters` |
| Lire le contenu du board sans Obsidian | lire le `.base` en YAML et les `.md` de `Board/`, tout est sur disque |

**Règle de course** : les `.base` sont réécrits par Obsidian depuis sa mémoire à chaud. Fermer l'onglet, vérifier la stabilité du hash, puis écrire.

Voir aussi [[kanban-bases-view]], [[core-templates]]
