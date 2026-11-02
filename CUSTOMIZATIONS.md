# Fork deployment maintenance

## Sub2API Extra image distribution

User-authorized deployment uses this fork's `ghcr.io/fjiangming/infinite-canvas` package. Preserve the upstream author, license, application interface, browser-local data model and direct API calls.

- `.github/workflows/docker-image.yml`: derive the package from `github.repository`; build on `main`, version tags and manual dispatch. Publish `latest` from the default branch, version tags and commit tags after both native amd64 and arm64 builds succeed. Validate the merged architecture manifest and prevent overlapping publication for the same ref.
- `docker-compose.yml`: use the fork's image instead of the original author's package.
- `README.md` and `docs/sub2api-extra-deployment.md`: document the fork image and the existing Extra root Compose update workflow, including the distinction between pulling an image and recreating a container.

No application source, authentication, model pricing, browser storage or dependency changes. Existing TODO and pending-test documents were reviewed; their application feature status is unchanged. The requested image build is performed by GitHub Actions. Deployment validation must check the image revision, anonymous package access, actual assets and the public domain before reporting success.
