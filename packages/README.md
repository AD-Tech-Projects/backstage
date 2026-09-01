# The Packages Folder

This is where your own applications and centrally managed libraries live, each
in a separate folder of its own.

From the start there's an `app` folder (for the frontend) and a `backend` folder
(for the Node backend), but you can also add more modules in here that house
your core additions and adaptations, such as themes, common React component
libraries, utilities, and similar.

## Current packages

| Package                         | Runs on | Docs                       |
| ------------------------------- | ------- | -------------------------- |
| [`app/`](app/README.md)         | `:9003` | Frontend React application |
| [`backend/`](backend/README.md) | `:7007` | Backend Node.js service    |

Both are Yarn workspaces (declared in the root `package.json`'s `workspaces`
field), so `yarn install` at the repo root links them together and
`yarn workspace app <script>` / `yarn workspace backend <script>` run scripts
scoped to just one of them.
