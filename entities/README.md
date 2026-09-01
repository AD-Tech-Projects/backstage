# entities

AD Tech's own catalog data, registered in `app-config.yaml` (`catalog.locations`)
alongside the generic `examples/` data. Migrated from the legacy
`backstage-ad_tech-legacy` project.

## Files

| File                                                      | Registers                                                                                                | Kind       |
| --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ---------- |
| `users/alvaro.user.yaml`                                  | Alvaro Alves, member of the `devs` group                                                                 | `User`     |
| `groups/devs.group.yaml`                                  | The `devs` team                                                                                          | `Group`    |
| `locations/node-backend-boilerplate-system.location.yaml` | Points at an externally-hosted `System` descriptor for the `Node_BackEnd_Boilerplate-TypeScript` project | `Location` |
| `locations/node-backend-boilerplate-api.location.yaml`    | Points at an externally-hosted `API` descriptor for the same project                                     | `Location` |

The two `locations/*.location.yaml` files are themselves catalog `Location`
entities — each one tells the catalog to additionally fetch and register a
`system.yml`/`api.yml` descriptor from
`raw.githubusercontent.com/AlvaroDavi5/Node_BackEnd_Boilerplate-TypeScript`.
For that fetch to succeed, `raw.githubusercontent.com` must be allow-listed
under `backend.reading.allow` in `app-config.yaml` (already configured).

A fifth external reference — the boilerplate project's own root
`catalog-info.yml` — is registered directly as a `type: url` location in
`app-config.yaml` rather than as a file here, since it isn't a local file.

## Adding more

Add a new `.yaml` file here, then add a matching entry under
`catalog.locations` in the root `app-config.yaml` (`type: file`, `target:` a
path relative to `packages/backend`, i.e. `../../entities/...`).
