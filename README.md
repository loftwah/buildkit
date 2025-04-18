# Docker Image Mirror

![Daily Mirror Status](https://github.com/loftwah/buildkit/actions/workflows/mirror-images.yml/badge.svg)
![All Versions Mirror Status](https://github.com/loftwah/buildkit/actions/workflows/mirror-all-images.yml/badge.svg)
![Weekly Tests Status](https://github.com/loftwah/buildkit/actions/workflows/test-images.yml/badge.svg)

This repository (`loftwah/buildkit`) mirrors Docker images to the GitHub Container Registry (GHCR) with precision and reliability, giving you fast access to key tools. Three workflows keep everything up to date and tested:

- **Daily Mirror**: Syncs `latest` tags every day for `buildkit`, `ruby`, `nginx`, and `node`, with an option to manually sync a specific version.
- **All Versions Mirror**: Manually fetches the latest versions plus two recent stable ones for all four images with a single click.
- **Weekly Tests**: Automatically tests all mirrored images every Sunday to ensure they're working correctly.

Here's what gets mirrored:

- `moby/buildkit` → `ghcr.io/loftwah/buildkit/buildkit`
- `ruby` → `ghcr.io/loftwah/buildkit/ruby`
- `nginx` → `ghcr.io/loftwah/buildkit/nginx`
- `node` → `ghcr.io/loftwah/buildkit/node`

## Pulling the Images

You'll need Docker installed (download it from [docker.com](https://www.docker.com/) if you don't have it). Open a terminal and use these commands to pull the `latest` versions, which are always available after the daily run:

```bash
docker pull ghcr.io/loftwah/buildkit/buildkit:latest  # BuildKit
docker pull ghcr.io/loftwah/buildkit/ruby:latest      # Ruby
docker pull ghcr.io/loftwah/buildkit/nginx:latest     # Nginx
docker pull ghcr.io/loftwah/buildkit/node:latest      # Node.js
```

### Specific Versions

- **Daily Workflow**: Automatically mirrors `latest` tags daily. For a specific version (e.g., `ruby:3.3.0`), use the manual trigger option below.
- **All Versions Workflow**: Pulls the latest version plus two prior stable ones when triggered manually. As of March 2024, typical versions might include:
  - **BuildKit**: `v0.13.0` (latest), `v0.12.5`, `v0.12.0`
  - **Ruby**: `3.3.0` (latest), `3.2.3`, `3.2.2`
  - **Nginx**: `1.25.4` (latest), `1.24.0`, `1.23.4`
  - **Node**: `21.7.1` (latest), `20.11.1`, `18.19.1`

To pull a specific version:

```bash
docker pull ghcr.io/loftwah/buildkit/ruby:3.3.0
```

If you get a "manifest unknown" error, it hasn't been mirrored yet—run the appropriate workflow first.

## Using the Images

Here's how to use them after pulling:

### BuildKit

For building Docker images efficiently. Start it:

```bash
docker run -d --name buildkitd --privileged ghcr.io/loftwah/buildkit/buildkit:latest
```

Then build a project:

```bash
docker buildx create --use --name mybuilder --driver remote tcp://localhost:1234
docker buildx build -t myimage:latest .
```

### Ruby

For running Ruby applications. Start it:

```bash
docker run -it ghcr.io/loftwah/buildkit/ruby:latest bash
```

Inside, run:

```bash
ruby -e 'puts "Ruby is ready to roll!"'
```

### Nginx

For hosting a web server. Start it:

```bash
docker run -d -p 80:80 ghcr.io/loftwah/buildkit/nginx:latest
```

Open `http://localhost` in your browser to see Nginx in action.

### Node.js

For JavaScript development. Start it:

```bash
docker run -it ghcr.io/loftwah/buildkit/node:latest bash
```

Inside, run:

```bash
node -e 'console.log("Node.js is up and running");'
```

## Workflows

### Daily Mirror (`mirror-images.yml`)

- **Runs**: Automatically every day at midnight UTC (12:00 AM universal time).
- **Purpose**: Keeps `latest` tags up to date for all images.
- **Manual Override**: Can sync a specific image version via workflow dispatch.

### All Versions Mirror (`mirror-all-images.yml`)

- **Runs**: Manually triggered only.
- **Purpose**: Syncs latest version plus two prior stable versions for each image.
- **Note**: Use this when you need specific version tags.

### Weekly Tests (`test-images.yml`)

- **Runs**: Automatically every Sunday at midnight UTC.
- **Purpose**: Ensures all mirrored images are working correctly.
- **Tests**: 
  - BuildKit: Verifies daemon startup
  - Ruby: Checks version command
  - Nginx: Tests configuration
  - Node.js: Validates version command
- **Notifications**:
  - Slack notifications (if configured via `SLACK_WEBHOOK_URL`)
  - Email notifications (if configured via `RESEND_API_KEY` and `NOTIFICATION_EMAIL`)
- **Manual Trigger**: Can be run on-demand via workflow dispatch.

## Contributing

Feel free to open issues or PRs if you'd like to improve this mirror setup.

## License

MIT License - feel free to use this in your own projects.