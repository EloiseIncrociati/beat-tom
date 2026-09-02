# Beat Tom

Application anti-procrastination sans culpabilisation. L'utilisateur declare le temps
reellement passe sur ses projets, et le visualise sur un calendrier.

Tom est la personnification de la procrastination, presentee comme un adversaire
externe : l'application ne reproche jamais rien a l'utilisateur.

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
| Etat client | Zustand, un store par domaine |
| Persistance locale | MMKV via adaptateur `StateStorage` |
| Etat serveur | TanStack Query |
| UI | gluestack UI v3 (composants copies) et NativeWind v4 |
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
| NestJS | derniere stable |
| GraphQL | Apollo, code-first, `autoSchemaFile` vers `src/schema.gql` versionne |
| Prisma | 7.x, adaptateur `@prisma/adapter-pg` obligatoire, `moduleFormat = "cjs"` |
| PostgreSQL | 17, image `postgres:17-alpine` |
| Configuration | `@nestjs/config`, echec au demarrage si une variable requise est absente |

## Prerequis de developpement

| Prerequis | Detail |
|---|---|
| Node.js | version LTS active |
| Docker Desktop | doit tourner : PostgreSQL 17 est conteneurise sur `localhost:5432` |
| Android | development build Expo installe sur un appareil ou un emulateur — Expo Go ne suffit pas, le projet utilise des modules natifs |

Environnement de developpement local uniquement. Il n'existe ni stage ni production.

## Structure du depot

| Dossier | Contenu |
|---|---|
| `beat-tom-mobile/` | application React Native (a creer) |
| `beat-tom-api/` | API NestJS (a creer) |
| `BeatTomObsidianVault/` | vault Obsidian, source de verite du projet |

Depot unique multi-projets, sans outillage de monorepo et sans workspaces npm.

## Documentation

Tout le reste vit dans le vault : cahier des charges, modele de donnees, contrat API,
strategie de synchronisation, design system, board des taches.

Point d'entree : `BeatTomObsidianVault/CLAUDE.md`, puis `BeatTomObsidianVault/Wiki/`.

Les commandes d'installation et de demarrage seront ajoutees ici quand les projets
`beat-tom-mobile/` et `beat-tom-api/` existeront.
