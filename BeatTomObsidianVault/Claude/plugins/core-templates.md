---
type: reference
tags: [claude, internal, plugins]
plugin-id: templates
status: actif
---

# Templates (core)

**id** `templates` · **Chemin** plugin core, activé dans `.obsidian/core-plugins.json` · **Données** `.obsidian/templates.json` + dossier `Templates/` · insère le contenu d'un fichier modèle dans la note courante, en résolvant les variables de date.

## Axes / Config

`.obsidian/templates.json` :

| Clé | Valeur Beat Tom | Rôle |
|---|---|---|
| `folder` | `Templates` | dossier scanné pour la liste des modèles |
| `dateFormat` | `YYYY-MM-DD` | format résolu par `{{date}}` |
| `timeFormat` | `HH:mm` | format résolu par `{{time}}` |

Variables résolues à l'insertion : `{{title}}`, `{{date}}`, `{{time}}`, et la forme explicite `{{date:FORMAT}}` qui l'emporte sur `dateFormat`.

Modèle disponible : `Templates/Tache Board.md`, frontmatter seul, sans corps.

Attention : `dateFormat` vaut `YYYY-MM-DD` (format des cartes), alors que les notes de session utilisent `DD-MM-YYYY` dans leur nom de fichier et leur frontmatter. Les deux conventions coexistent volontairement — la carte est triée par date ISO, le journal de session est lu par un humain.

## Frontmatter attendu

Le modèle de carte produit exactement :

```yaml
---
id: 
platform: 
type: feature
lot: 
status: To Do
priority: Medium
date: {{date:YYYY-MM-DD}}
assignee: Claude Code
---
```

Les champs vides sont à remplir à la création. Une carte sans `status` tombe en `Uncategorized` sur le board.

## Opérations

| Action | Comment |
|---|---|
| Créer une carte sans l'interface | écrire directement `Board/<titre>.md` avec le frontmatter résolu, date en dur au format `YYYY-MM-DD`. Ne pas recopier `{{date:...}}`, rien ne le résoudra hors interface. |
| Modifier le modèle | éditer `Templates/Tache Board.md`, c'est un `.md` ordinaire, éditable à tout moment |
| Ajouter un modèle | créer un `.md` dans `Templates/`, il apparaît dans la liste sans redémarrage |
| Changer le dossier de modèles | éditer `folder` dans `.obsidian/templates.json` — **règle de course applicable** |

Voir aussi [[core-bases]], [[kanban-bases-view]]
