# Backstage AD Tech Solutions

A [Backstage](https://backstage.io) instance (v1.53.0) — an internal developer
portal for AD Tech Solutions. Catalogs software components and APIs, hosts TechDocs,
and provides scaffolding templates for new projects.

This app was generated with `@backstage/create-app` and uses the **new frontend
system** (`@backstage/frontend-defaults`) on the client and the **new backend
system** (`@backstage/backend-defaults`) on the server.

## Project layout

| Path                                              | What it is                                                             |
| ------------------------------------------------- | ---------------------------------------------------------------------- |
| [`packages/app/`](packages/app/README.md)         | The frontend React app (served on `:9003`)                             |
| [`packages/backend/`](packages/backend/README.md) | The backend service (served on `:7007`)                                |
| [`plugins/`](plugins/README.md)                   | Home for any custom, in-repo Backstage plugins                         |
| [`examples/`](examples/README.md)                 | Sample catalog entities and a scaffolder template used for local dev   |
| [`entities/`](entities/README.md)                 | AD Tech Solutions' own catalog entities: org structure + Node.js Backend Boilerplate integration |
| `app-config.yaml`                                 | Main app configuration (dev) — see [Configuration](#configuration)     |
| `app-config.production.yaml`                      | Overrides merged in only when `NODE_ENV=production`                    |
| `catalog-info.yaml`                               | Registers this repo itself as a `Component` in the catalog             |
| `backstage.json`                                  | Records the Backstage release version this app was created from        |

## Prerequisites

- **Node.js 20 or 22** (Active LTS). This project pins `22.16.0` via
  [asdf](https://asdf-vm.com) in `.tool-versions`. Node 24+ is not officially
  supported by Backstage yet, even though it runs.
- **Yarn 4** (Corepack/Yarn Berry) — already vendored in `.yarn/releases`, no
  global install needed.

## Running the project

```sh
# 1. Install dependencies (uses Yarn workspaces: packages/* + plugins/*)
yarn install

# 2. Start the app — runs the backend (:7007) and frontend (:9003) together,
#    with hot reload for both.
yarn start
```

Then open **http://localhost:9003**. Sign in with the pre-configured **guest**
auth provider (no credentials needed in dev).

Other useful scripts (see root [`package.json`](package.json)):

```sh
yarn build:all         # Production build of every workspace package
yarn build:backend     # Production build of just the backend
yarn build-image       # Build the backend's Docker image
yarn test              # Run unit tests (Jest) for changed packages
yarn test:all          # Run unit tests with coverage, all packages
yarn test:e2e          # Run Playwright end-to-end tests
yarn lint              # Lint files changed since origin/main
yarn lint:all          # Lint the whole repo
yarn fix               # Auto-fix lint issues
yarn tsc               # Type-check the whole repo
yarn new               # Scaffold a new plugin/module into plugins/
```

## Configuration

Backstage reads `app-config.yaml` (and merges `app-config.production.yaml` in
prod). Key defaults in this repo:

- **Database**: SQLite in-memory (`better-sqlite3`) for local dev — no setup
  required. Swap to Postgres for production (see comments in
  `app-config.production.yaml`).
- **Auth**: `guest` provider only — anyone can sign in without credentials.
  Fine for local dev; replace before deploying (see
  https://backstage.io/docs/auth/).
- **Catalog**: auto-discovers entities declared under `packages: all` plus
  whatever's registered via `catalog-info.yaml` / the `examples/` and
  `entities/` locations.
- **Organization**: `organization.name` is set to `AD Tech Solutions`.
- **Ports**: frontend on `9003`, backend on `7007` (`app.baseUrl` /
  `backend.cors.origin` match).
- **GitHub integration**: reads a Personal Access Token from the `GITHUB_TOKEN`
  environment variable (`integrations.github`) — required for the scaffolder's
  GitHub actions and for anything under `catalog.locations` that references a
  private repo.

Do not commit real secrets into `app-config.yaml` — use `${ENV_VAR}`
substitution (Backstage resolves these from the process environment at
startup).

## Code style

Formatting is enforced by Prettier via `.prettierrc` (tabs, single quotes,
`printWidth: 160`) and mirrored in `.editorconfig` for editors that don't run
Prettier directly. This overrides the `@backstage/cli` default (spaces) — run
`yarn prettier:check` / `yarn fix` to apply it.

## Adding functionality

- **New custom plugin**: `yarn new` and follow the prompt, or hand-create a
  package under `plugins/` (see [`plugins/README.md`](plugins/README.md)).
- **New frontend feature/plugin**: add it to the `features` array in
  `packages/app/src/App.tsx`; customize navigation using the `NavContentBlueprint`
  pattern in `packages/app/src/modules/nav/Sidebar.tsx`.
- **New backend plugin**: add a `backend.add(import('@backstage/plugin-...'))`
  line in `packages/backend/src/index.ts`.
- **New catalog entity**: add a `.yaml` file under `entities/` and register it
  in `app-config.yaml` under `catalog.locations` (see [`entities/README.md`](entities/README.md)).

## Learn more

- Backstage docs: https://backstage.io/docs/
- Software catalog: https://backstage.io/docs/features/software-catalog/configuration
- Authentication providers: https://backstage.io/docs/auth/
