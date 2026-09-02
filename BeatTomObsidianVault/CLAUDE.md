## Purpose

Ce vault est la source de vérité unique du projet Beat Tom. Un agent qui reprend le projet doit pouvoir continuer depuis ce vault seul, sans historique de conversation, sans contexte oral, sans accès au dépôt de code.

Tout ce qui n'est pas écrit ici n'existe pas.

## Bootstrap, ordre de lecture

| Priorité | Fichier | Quand |
|---|---|---|
| 1 | `Claude/plugins/<plugin>.md` | avant toute action sur ce plugin |
| 2 | `Beat Tom Board.base` et `Board/` | si une tâche ou un bug est à tracker |
| 3 | `Claude/log/audits/` | si audit ou régression |
| 4 | `Claude/log/sessions/` | jamais, sauf audit ou régression |

## Vault structure

```
BeatTomObsidianVault/
├── CLAUDE.md                 <- ce fichier, contrat de fonctionnement du vault
├── Beat Tom Board.base       <- kanban Bases, à la racine du vault
├── Board/                    <- 40 cartes, 1 .md par carte, `<id> <titre>.md`
├── Wiki/                     <- doc libre humaine
│   ├── Fonctionnement du vault.md
│   ├── Liens utiles.md
│   ├── Cahier des charges.md
│   ├── Modele de donnees.md
│   ├── Contrat API.md
│   ├── Strategie de synchronisation.md
│   ├── Design system.md
│   └── Mascotte Tom.md
├── Templates/
│   └── Tache Board.md
├── Upload/                   <- pièces jointes (auto)
├── Claude/
│   ├── plugins/
│   │   ├── kanban-bases-view.md
│   │   ├── tasks.md
│   │   ├── core-templates.md
│   │   └── core-bases.md
│   ├── log/
│   │   ├── audits/
│   │   └── sessions/         <- 01-09-2026.md, 02-09-2026.md
│   └── Prompts/              <- prompts réutilisables (rôles et modes)
└── .obsidian/                <- configuration du vault
    ├── app.json              <- pièces jointes dans Upload/, wikilinks imposés
    ├── appearance.json       <- thème Minimal, mode sombre, accent #E94FC7
    ├── core-plugins.json     <- plugins core actifs, dont templates et bases
    ├── community-plugins.json<- liste des plugins communautaires installés
    └── templates.json        <- dossier Templates/, dates YYYY-MM-DD
```

Les dossiers vides du vault portent un `.gitkeep` pour survivre au clone :
`Upload/`, `Claude/Prompts/`, `Claude/log/audits/`.

Non versionnés : `.obsidian/workspace.json` (état d'interface, propre à la machine),
le code des plugins et `.obsidian/themes/` (à réinstaller après clone). Les
`.obsidian/plugins/*/data.json` sont en revanche versionnés : c'est de la
configuration de vault, pas du code tiers.

### Rôle des dossiers

| Dossier | Rôle |
|---|---|
| `Board/` | une note par carte, nommée `<id> <titre>.md`. Le kanban n'est qu'une vue de ces notes. |
| `Wiki/` | documentation humaine durable, écrite par Liz. Claude ne remplit pas ces fichiers d'un contenu inventé. |
| `Templates/` | modèles instanciés par le plugin core Templates. |
| `Upload/` | pièces jointes, alimenté automatiquement par Obsidian. |
| `Claude/plugins/` | fiches opératoires des plugins fonctionnels : comment agir sans passer par l'interface. |
| `Claude/log/audits/` | rapports d'audit et analyses de régression. |
| `Claude/log/sessions/` | journal de fin de session. Écrit systématiquement, relu quasi jamais. |
| `Claude/Prompts/` | prompts réutilisables (rôles, modes de travail). |

## Write rules

| Événement | Action |
|---|---|
| Bug ou tâche de priorité High | créer `Board/<titre>.md` |
| Tâche déplacée | éditer `status` ou `priority` dans le frontmatter |
| Fin de session | `Claude/log/sessions/DD-MM-YYYY.md` |
| Audit | `Claude/log/audits/YYYY-MM-DD-<sujet>.md` |
| Schéma Prisma modifié | mettre à jour `Wiki/Modele de donnees.md` |
| Contrat GraphQL modifié | mettre à jour `Wiki/Contrat API.md` |
| Fichier créé | mettre à jour la file map immédiatement |
| Action sur un plugin | lire d'abord `Claude/plugins/<plugin>.md` |

> **Régression constatée sur T-001.** La règle du journal de fin de session n'a
> pas été honorée : les deux premières sessions n'ont laissé aucune note, et les
> journaux `Claude/log/sessions/01-09-2026.md` et `02-09-2026.md` ont dû être
> reconstruits rétroactivement depuis les dates de fichiers et l'historique Git.
> Les décisions de la session du 01-09 sont définitivement perdues.
>
> Une règle qui dépend de la mémoire de l'agent en fin de contexte n'est pas
> tenue. Le journal de session doit donc désormais figurer comme **dernier
> livrable explicite de chaque prompt de session**, au même titre que les autres
> tâches, et non comme une convention de fond.

## Plugins, map et usage

| Plugin | id | Données | Doc |
|---|---|---|---|
| Kanban Bases View | `kanban-bases-view` | `Beat Tom Board.base` (racine du vault) | [[kanban-bases-view]] |
| Tasks | `obsidian-tasks-plugin` | `.obsidian/plugins/obsidian-tasks-plugin/data.json` | [[tasks]] |
| Templates (core) | `templates` | `.obsidian/templates.json` + `Templates/` | [[core-templates]] |
| Bases (core) | `bases` | fichiers `.base` du vault | [[core-bases]] |

### Plugins cosmétiques — NON documentés, ne pas toucher

- `obsidian-icon-folder`
- `flexplorer`
- `obsidian-minimal-settings`

Ils ne gèrent ni notes ni fichiers. Aucune fiche dans `Claude/plugins/`, aucune édition de leur `data.json`.

### Thème

| Élément | Valeur |
|---|---|
| Thème | Minimal |
| Mode | `obsidian` (sombre) |
| Couleur d'accent | `#E94FC7` (accent magenta du projet, à confirmer depuis Figma) |

### Règle de course (plugins)

Obsidian réécrit `.base`, `data.json` et `.obsidian/*.json` depuis sa mémoire à chaud, donc tout édit externe est écrasé.

| Type de fichier | Quand écrire |
|---|---|
| Donnée `.md` | à tout moment, Obsidian recharge depuis le disque |
| Configuration `.obsidian/*.json`, `data.json`, `.base` | fermer l'onglet, vérifier que le hash est stable, puis écrire |

### Frontmatter tâche ou carte

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

| Propriété | Valeurs |
|---|---|
| `id` | `F-XXX` mobile, `B-XXX` backend, `T-XXX` transverse, `BUG-XXX` anomalie |
| `platform` | `mobile`, `api`, `infra` |
| `type` | `feature`, `bug` |
| `lot` | `0`, `1`, `2`, `3`, `4` |
| `status` | `Roadmap`, `To Do`, `Doing`, `Done` |
| `priority` | `High`, `Medium`, `Low` |
| `assignee` | `Liz`, `Claude Code` |

Le préfixe de `id` porte déjà la surface, donc `platform` sert au filtrage et non à la lecture.

## Agent scope

| Acteur | Périmètre | Hors périmètre |
|---|---|---|
| Liz (Éloïse Incrociati) | décisions produit et architecture, validation de chaque étape | rien |
| Claude Code | implémentation dans `beat-tom-mobile/` et `beat-tom-api/`, écriture du vault | déploiement, gestion des secrets, décisions produit |
| Claude (chat) | architecture, découpage en cartes, cours pédagogiques | écriture directe dans le dépôt |

**Règle d'escalade** : toute décision qui contredit le cahier des charges ou les principes de conception non négociables remonte à Liz avant implémentation, jamais après.

### Liens utiles

| Cible | URL |
|---|---|
| Dépôt GitHub | https://github.com/EloiseIncrociati/beat-tom.git |
| Health check dev | `POST http://localhost:3000/graphql`, query `health` |
| Base de données dev | `localhost:5432`, PostgreSQL 17 conteneurisé |
| Cahier des charges | [[Cahier des charges]] |
| Stage | n'existe pas |
| Prod | n'existe pas |

## Note format

```markdown
---
date: DD-MM-YYYY
type: bug|decision|spec|session|constraint|architecture
tags: [beat-tom, mobile]
---

# Titre

Corps de la note. Toujours des [[wikilinks]], jamais de liens markdown classiques.
```
