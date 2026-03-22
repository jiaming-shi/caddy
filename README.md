# Custom Caddy Image

This repository builds and publishes a custom Caddy image with the `github.com/caddy-dns/cloudflare` module included.

## Published Image

Images are published to:

```text
ghcr.io/<owner>/caddy
```

Tags:

- `<caddy-version>` for each detected stable upstream Caddy release
- `latest` for the most recent stable upstream Caddy release

## Automation

The GitHub Actions workflow does the following:

1. Runs on a schedule every 6 hours.
2. Checks the latest stable release from `caddyserver/caddy`.
3. Skips the build if the same version is already present in GHCR.
4. Builds a multi-platform image for `linux/amd64` and `linux/arm64`.
5. Pushes the resulting image to GitHub Container Registry.

Manual triggering is also available through `workflow_dispatch`. You can optionally provide a stable Caddy version.

## Local Build

Build a local image with a specific Caddy version:

```bash
docker build --build-arg CADDY_VERSION=2.10.2 -t custom-caddy:2.10.2 .
```

Verify that the Cloudflare DNS module is included:

```bash
docker run --rm custom-caddy:2.10.2 caddy list-modules | grep cloudflare
```

## Pull From GHCR

Pull the latest image:

```bash
docker pull ghcr.io/<owner>/caddy:latest
```

Pull a specific version:

```bash
docker pull ghcr.io/<owner>/caddy:<version>
```

## Repository Bootstrap

If this repository has not been published yet, create the GitHub repository and push it with:

```bash
gh repo create caddy --public --source=. --remote=origin --push
```
