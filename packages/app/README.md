# app

The Backstage frontend — a React single-page app built with the **new
frontend system** (`@backstage/frontend-defaults`). Serves the developer
portal UI on `http://localhost:9003` in development.

## Structure

```
packages/app/
├── public/            Static assets (favicons, manifest.json, index.html shell)
├── src/
│   ├── index.tsx      Entry point — mounts the app created in App.tsx
│   ├── App.tsx         App composition root: registers plugin "features"
│   ├── App.test.tsx    Smoke test that the app renders
│   ├── setupTests.ts   Jest/Testing Library global setup
│   └── modules/
│       └── nav/        Custom sidebar navigation module
│           ├── index.ts        Exports the nav module as a frontend feature
│           ├── Sidebar.tsx     Sidebar layout/composition
│           ├── SidebarLogo.tsx Logo + collapse toggle shown at the top of the sidebar
│           ├── LogoFull.tsx    Full wordmark logo (expanded sidebar)
│           └── LogoIcon.tsx    Icon-only logo (collapsed sidebar)
└── e2e-tests/
    └── app.test.ts     Playwright end-to-end test (loads the app in a real browser)
```

## How it fits together

`src/App.tsx` is the single composition point:

```ts
export default createApp({
	features: [catalogPlugin, navModule],
});
```

Every frontend plugin (catalog, techdocs, search, scaffolder UI, etc.) is
registered here as a "feature". To add a plugin's UI, install its package as
a dependency and add it to the `features` array. To customize the sidebar,
edit `src/modules/nav/`.

## Configuration

The app reads config from the root `app-config.yaml` (merged with
`app-config.production.yaml` in production) — not from anything inside this
package. See the root [README](../../README.md#configuration).

## Scripts

Run from the repo root, or `cd packages/app` first:

```sh
yarn workspace app start   # Dev server with hot reload (:9003)
yarn workspace app build   # Production build (outputs to dist/)
yarn workspace app test    # Jest unit tests
yarn workspace app lint    # ESLint
```

End-to-end tests (Playwright) live in `e2e-tests/` and run from the repo root
via `yarn test:e2e`, not from within this package.
