# Fork deployment maintenance

## Main-domain subdirectory mounting

The user requested a same-origin `aihub.fo2.us.ci/canvas` entry in the existing server configuration. Preserve the light-theme and header changes already present in commit `2b207f1`.

- `web/index.html` and `web/vite.config.ts`: add a stable HTML base element, relative public references and relative build assets. The reverse proxy replaces only the base element to mount the same image below `/canvas/`; root-domain HTML retains `/`.
- `web/src/constant/env.ts`, `web/src/router.tsx`, `web/src/components/layout/app-top-nav.tsx`, `web/src/lib/canvas/plugin-loader.ts`: derive the router basename, logo and local-plugin URLs from the document base. The mount root opens the canvas library; the standalone root retains its home page.
- `CHANGELOG.md`, `docs/content/docs/progress/pending-test.mdx` and `docs/sub2api-extra-deployment.md`: record subdirectory support, the Nginx integration and acceptance checks. Existing TODO items were reviewed and retained.

No model gateway or personal key changes. Browser storage remains scoped by origin; data from the independent canvas domain does not automatically move to the main domain. Publication and deployment verification are recorded in Sub2API Extra's maintenance ledger.

## Sub2API Extra image distribution

User-authorized deployment uses this fork's `ghcr.io/fjiangming/infinite-canvas` package. Preserve the upstream author, license, application interface, browser-local data model and direct API calls.

- `.github/workflows/docker-image.yml`: derive the package from `github.repository`; build on `main`, version tags and manual dispatch. Publish `latest` from the default branch, version tags and commit tags after both native amd64 and arm64 builds succeed. Validate the merged architecture manifest and prevent overlapping publication for the same ref.
- `docker-compose.yml`: use the fork's image instead of the original author's package.
- `README.md` and `docs/sub2api-extra-deployment.md`: document the fork image and the existing Extra root Compose update workflow, including the distinction between pulling an image and recreating a container.

No application source, authentication, model pricing, browser storage or dependency changes. Existing TODO and pending-test documents were reviewed; their application feature status is unchanged. The requested image build is performed by GitHub Actions. Deployment validation must check the image revision, anonymous package access, actual assets and the public domain before reporting success.
