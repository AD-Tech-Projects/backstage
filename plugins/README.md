# The Plugins Folder

This is where your own plugins and their associated modules live, each in a
separate folder of its own.

If you want to create a new plugin here, go to your project root directory, run
the command `yarn new`, and follow the on-screen instructions.

You can also check out existing plugins on [the plugin marketplace](https://backstage.io/plugins)!

## Status

Empty for now — no custom plugins have been created yet. This folder is a
placeholder so the `plugins/*` Yarn workspace glob in the root `package.json`
has somewhere to resolve once you add one.

## Adding a plugin

```sh
yarn new
```

This runs the Backstage CLI scaffolder, which asks what kind of plugin
(frontend, backend, common, etc.) and creates a new folder here with its own
`package.json`, source, and tests, already wired into the Yarn workspace. After
creating a **backend** plugin you still need to register it manually in
[`packages/backend/src/index.ts`](../packages/backend/README.md) via
`backend.add(...)`; a **frontend** plugin needs to be added to the `features`
array in [`packages/app/src/App.tsx`](../packages/app/README.md).
