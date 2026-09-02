@BeatTomObsidianVault/CLAUDE.md

# Beat Tom · DEV

## Environnement

Dev local uniquement.

| Élément | Valeur |
|---|---|
| Health check | `POST http://localhost:3000/graphql`, query `health` |
| Base de données | PostgreSQL 17 conteneurisé, `localhost:5432` |
| Prérequis session | Docker Desktop doit tourner |
| Stage | n'existe pas |
| Prod | n'existe pas |

Aucun accès stage ni prod. Ces environnements n'existent pas : ne pas inventer d'URL, ne pas supposer de pipeline de déploiement.

## Working directory

| Élément | Chemin |
|---|---|
| Claude Code ouvert depuis | `c:\Users\Liz Ombréclipse\Documents\Code\BT` |
| Mobile | `beat-tom-mobile/` (à créer) |
| API | `beat-tom-api/` (à créer) |
| Vault | `BeatTomObsidianVault/` |
| Hors périmètre | déploiement, gestion des secrets, décisions produit |

## Token discipline

```
On resume : bootstrap vault + read TODO ONLY
NEVER read a file to "verify" if path is known
NEVER grep/read after edit to confirm
NEVER re-read a file already read this session
Edit in one pass, no read then edit then read
File map below is authoritative, no exploration
When creating a file, update File map immediately in this CLAUDE.md
```

## Stack

### Mobile

| Brique | Version |
|---|---|
| Expo SDK | 57 |
| React Native | 0.86, New Architecture, bridgeless |
| React | 19.x |
| Moteur JS | Hermes |
| TypeScript | strict, aucun `any` y compris dans les tests |
| Navigation | Expo Router |
| État client | Zustand, un store par domaine |
| Persistance locale | MMKV via adaptateur `StateStorage` |
| État serveur | TanStack Query |
| UI | gluestack UI v3 (composants copiés) et NativeWind v4 |
| Animation | Reanimated 4.x |
| Gestes | React Native Gesture Handler 2.3x |
| Feuilles | `@gorhom/bottom-sheet` |
| Calendrier | react-native-calendars, injection par `dayComponent` |
| Listes | FlashList v2 |
| Dates | date-fns |
| Graphiques | react-native-svg |

### API

| Brique | Version |
|---|---|
| NestJS | dernière stable |
| GraphQL | Apollo, approche code-first, `autoSchemaFile` vers `src/schema.gql` versionné |
| Prisma | 7.x, adaptateur `@prisma/adapter-pg` obligatoire, `moduleFormat = "cjs"` |
| PostgreSQL | 17, image `postgres:17-alpine` |
| Configuration | `@nestjs/config`, échec au démarrage si une variable requise est absente |

### Outillage

GitHub Actions, EAS Build, EAS Update, EAS Submit, Maestro, Jest, React Native Testing Library.

## File map

Arborescence cible issue du cahier des charges, pas un état constaté. Les dossiers annotés `(à créer)` n'existent pas encore. Mise à jour immédiate à chaque fichier créé, y compris en cours de session.

```
beat-tom/
├── beat-tom-mobile/                  <- application React Native (à créer)
│   ├── app/                          <- routes Expo Router
│   │   ├── (tabs)/                   <- index.tsx calendrier, stats.tsx, _layout.tsx
│   │   ├── modals/                   <- add-activity.tsx, add-project.tsx
│   │   └── _layout.tsx
│   ├── src/
│   │   ├── features/                 <- découpage par domaine métier
│   │   │   ├── projects/             <- store.ts, types.ts, queries.ts, utils.ts
│   │   │   ├── activities/           <- store.ts, types.ts, queries.ts, aggregations.ts
│   │   │   ├── stats/                <- selectors.ts
│   │   │   └── sync/                 <- outbox.ts, reconcile.ts
│   │   ├── components/               <- calendar/, sheets/, ui/ (gluestack copiés)
│   │   ├── storage/                  <- mmkv.ts, secure.ts
│   │   ├── api/                      <- client.ts, generated/
│   │   └── constants/                <- theme.ts
│   └── __tests__/
├── beat-tom-api/                     <- API NestJS (à créer)
│   ├── prisma/                       <- schema.prisma, migrations/
│   ├── prisma.config.ts
│   ├── src/
│   │   ├── prisma/                   <- prisma.service.ts, prisma.module.ts
│   │   ├── users/
│   │   ├── projects/                 <- resolver, service, module, models/, dto/
│   │   ├── activities/
│   │   ├── sync/
│   │   ├── schema.gql                <- généré et versionné
│   │   └── main.ts
│   └── test/
├── BeatTomObsidianVault/             <- suivi de projet
│   ├── CLAUDE.md
│   ├── Beat Tom Board.base           <- kanban Bases, 4 vues (Status, Bugs, Priorite, Vue)
│   ├── Board/                        <- 40 cartes de roadmap, 1 .md par carte.
│   │                                    Nommage `<id> <titre>.md` (ex. `F-006 création
│   │                                    de projet.md`). Ne pas lister ici, la source
│   │                                    est le dossier et le board.
│   ├── Wiki/                         <- Fonctionnement du vault.md, Liens utiles.md,
│   │                                    Cahier des charges.md, Modele de donnees.md,
│   │                                    Contrat API.md, Strategie de synchronisation.md,
│   │                                    Design system.md, Mascotte Tom.md
│   ├── Templates/                    <- Tache Board.md
│   ├── Upload/                       <- pièces jointes (auto, vide, .gitkeep)
│   ├── Claude/
│   │   ├── plugins/                  <- kanban-bases-view.md, tasks.md,
│   │   │                                core-templates.md, core-bases.md
│   │   ├── log/                      <- audits/ (vide, .gitkeep),
│   │   │                                sessions/ (01-09-2026.md, 02-09-2026.md)
│   │   └── Prompts/                  <- prompts réutilisables (vide, .gitkeep)
│   └── .obsidian/                    <- app.json, appearance.json, core-plugins.json,
│                                        community-plugins.json, templates.json
│                                        (plugins/, themes/, workspace.json ignorés par Git)
├── .claude/                          <- config Claude Code
│   ├── settings.local.json           <- LOCAL UNIQUEMENT, JAMAIS DANS LE DÉPÔT.
│   │                                    Exclu par la config Git globale de la machine
│   │                                    (`~/.config/git/ignore`), pas par le .gitignore
│   │                                    du projet. Le fichier existe sur le disque de
│   │                                    Liz mais un clone ne le recevra pas : ne jamais
│   │                                    supposer sa présence, ne jamais y mettre une
│   │                                    information dont le projet dépend.
│   └── skills/
│       └── beat-tom-design/
│           └── SKILL.md
├── CLAUDE.md                         <- ce fichier
├── docker-compose.yml                <- PostgreSQL 17 de développement (à créer)
├── .gitattributes                    <- `* text=auto eol=lf`, fins de ligne normalisées
├── .gitignore                        <- 3 surfaces + secrets + système + Obsidian
└── README.md                         <- présentation, stack, prérequis de dev
```

Dépôt unique multi-projets, sans outillage de monorepo, sans workspaces npm. La convention `beat-tom-<surface>` pourra migrer vers une structure `apps/` le jour où un partage réel de code apparaît, sans casser d'ici là les chemins Expo, EAS et Android.

## Rules

- Ne jamais scanner le dépôt. La file map fait autorité.
- La file map est un document vivant : mise à jour immédiate à chaque création ou suppression de fichier.
- Fichiers de routes et de modules en kebab-case (`add-activity.tsx`, `prisma.service.ts`).
- Composants React en PascalCase. Hooks en camelCase préfixés `use`.
- Dossiers de features au pluriel et en minuscules (`activities/`, `projects/`).
- Découpage par domaine métier, jamais par type technique. Un dossier de feature regroupe store, types et calculs.
- Pas de versioning d'URL sur l'API. Le contrat est versionné par `src/schema.gql` en Git, tout changement doit être visible dans le différentiel.
- Identifiants générés côté client en UUID v4, jamais par la base.
- Aucune mutation ne renvoie `void` : toute mutation renvoie l'entité résultante.
- Les types GraphQL sont distincts des types Prisma, y compris quand ils sont identiques.
- Le champ `date` d'une session est une chaîne `YYYY-MM-DD`, jamais un horodatage. C'est un contrat partagé entre le calendrier, le stockage local, l'API et la base.
- Suppressions logiques via `deletedAt`, jamais physiques.
- Commits conventionnels préfixés par l'identifiant de carte : `feat(F-006): project creation form`.
- Plan d'architecture énoncé avant d'écrire du code, jamais l'inverse.
- Choix d'outillage : le projet privilégie l'outil le plus établi de l'écosystème, même quand une solution plus simple suffirait. Contrepartie obligatoire, chaque fois que c'est le cas, la documentation doit indiquer qu'une alternative plus simple existait et pourquoi les deux coexistent dans l'écosystème.
