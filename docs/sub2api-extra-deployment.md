# Sub2API Extra deployment

This fork publishes `ghcr.io/fjiangming/infinite-canvas:latest` from `main` using GitHub Actions. The image supports Linux amd64 and arm64. The workflow also publishes `sha-<full commit>` tags for repeatable deployment and rollback. Version tag builds do not replace the default-branch `latest` image.

Enable Actions in a newly created fork before the first build. Make the GHCR package public after its first publication so the deployment server can pull without GitHub credentials. A successful push is not evidence that an image build has finished.

The Sub2API Extra repository owns `infinite-canvas/compose.yaml` and the Nginx integration. Add that module to the existing root `include` list and append `infinite-canvas` to the server's existing `COMPOSE_PROFILES`, retaining every previously enabled service. The service uses host loopback port 9877 and container port 3000; it needs no database or persistent volume.

Run updates from the existing Extra root directory:

```bash
docker compose --env-file compose.services.env pull
docker compose --env-file compose.services.env up -d --no-build
```

The first command pulls all enabled services, including this fork's canvas image. The second applies the downloaded images. To update only the canvas:

```bash
docker compose --env-file compose.services.env pull infinite-canvas
docker compose --env-file compose.services.env up -d --no-build --no-deps infinite-canvas
```

The production page is served at `https://canvas.fo2.us.ci/canvas` through the existing Nginx and Cloudflare ingress. The canvas Nginx site clears Sub2API login query parameters before serving the application and can forward the user's API requests to the fixed Sub2API gateway. This is a static browser workspace, without Sub2API SSO or account-based data storage. Each user configures their own Sub2API API key and available models. Canvas data and keys remain in that user's browser; optional WebDAV sync retains upstream behavior.
