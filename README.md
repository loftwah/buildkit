# Docker Image Mirror

This repository (`loftwah/buildkit`) mirrors Docker images to the GitHub Container Registry (GHCR) with precision and reliability, giving you fast access to key tools. Two workflows keep everything up to date:

- **Daily Mirror**: Syncs `latest` tags every day for `buildkit`, `ruby`, `nginx`, and `node`, with an option to manually sync a specific version.
- **All Versions Mirror**: Manually fetches the latest versions plus two recent stable ones for all four images with a single click.

Here’s what gets mirrored:

- `moby/buildkit` → `ghcr.io/loftwah/buildkit/buildkit`
- `ruby` → `ghcr.io/loftwah/buildkit/ruby`
- `nginx` → `ghcr.io/loftwah/buildkit/nginx`
- `node` → `ghcr.io/loftwah/buildkit/nodejs`

## Pulling the Images

You’ll need Docker installed (download it from [docker.com](https://www.docker.com/) if you don’t have it). Open a terminal and use these commands to pull the `latest` versions, which are always available after the daily run:

```bash
docker pull ghcr.io/loftwah/buildkit/buildkit:latest  # BuildKit
docker pull ghcr.io/loftwah/buildkit/ruby:latest      # Ruby
docker pull ghcr.io/loftwah/buildkit/nginx:latest     # Nginx
docker pull ghcr.io/loftwah/buildkit/nodejs:latest    # Node.js
```

### Specific Versions

- **Daily Workflow**: Automatically mirrors `latest` tags daily. For a specific version (e.g., `ruby:3.3.0`), use the manual trigger option below.
- **All Versions Workflow**: Pulls the latest version plus two prior stable ones when triggered manually. As of February 2025, you might see:
  - **BuildKit**: `v0.12.5` (latest), `v0.12.0`, `v0.11.6`.
  - **Ruby**: `3.3.0` (latest), `3.2.2`, `3.1.4`.
  - **Nginx**: `1.25.3` (latest), `1.24.0`, `1.22.1`.
  - **Node**: `20.11.0` (latest), `18.19.0`, `16.20.2`.

To pull a specific version:

```bash
docker pull ghcr.io/loftwah/buildkit/ruby:3.3.0
```

If you get a “manifest unknown” error, it hasn’t been mirrored yet—run the appropriate workflow first.

## Using the Images

Here’s how to use them after pulling:

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
docker run -it ghcr.io/loftwah/buildkit/nodejs:latest bash
```

Inside, run:

```bash
node -e 'console.log("Node.js is up and running");'
```

## Workflows

### Daily Mirror (`mirror-images.yml`)

- **Runs**: Automatically every day at midnight UTC (12:00 AM universal time).
- **Purpose**: Keeps `latest` tags current for all four images.
- **Manual Option**: Trigger it yourself to sync `latest` for all images or a specific version.

#### Manual Trigger

1. Visit [https://github.com/loftwah/buildkit/actions](https://github.com/loftwah/buildkit/actions).
2. Select “Mirror Docker Images.”
3. Click “Run workflow.”
4. **Optional**: Enter an image (e.g., `ruby:3.3.0`) to sync just that version. Leave blank to sync all `latest` tags.
5. Keep the branch as `main`.
6. Click “Run workflow” and wait a few minutes.

### All Versions Mirror (`mirror-all-images.yml`)

- **Runs**: Only when you trigger it manually.
- **Purpose**: Fetches the latest version plus two prior stable ones for all four images, directly from Docker Hub.

#### Manual Trigger

1. Visit [https://github.com/loftwah/buildkit/actions](https://github.com/loftwah/buildkit/actions).
2. Select “Mirror All Docker Images.”
3. Click “Run workflow.”
4. Leave the input box empty—no need to type anything.
5. Keep the branch as `main`.
6. Click “Run workflow” and wait a few minutes.

## What Happens If You Pull an Unmirrored Image

If you try to pull an image that hasn’t been mirrored yet (e.g., `ghcr.io/loftwah/buildkit/ruby:3.3.0`):

- Command: `docker pull ghcr.io/loftwah/buildkit/ruby:3.3.0`
- Result: You’ll see “manifest unknown” or “pull access denied”—it’s not there yet.
- **Solution**: Run the right workflow:
  - For `latest`: Use “Mirror Docker Images” with no input.
  - For specific versions: Use “Mirror Docker Images” with the tag (e.g., `ruby:3.3.0`).
  - For latest + older versions: Use “Mirror All Docker Images.”

Check available tags at [https://github.com/loftwah/buildkit/packages](https://github.com/loftwah/buildkit/packages)—click a package to see what’s been mirrored.

## Files in This Repository

```
.
├── .github
│   └── workflows
│       ├── mirror-images.yml     # Daily latest sync
│       └── mirror-all-images.yml # Manual all versions sync
└── README.md                     # This documentation
```

## Permissions

Both workflows require:

- `packages: write` - To push images to GHCR.
- `contents: read` - To access workflow files.

## Slack Notifications (Optional)

To enable notifications:

1. Create a Slack webhook (search “Slack webhook setup” for instructions).
2. Go to [https://github.com/loftwah/buildkit/settings/secrets/actions](https://github.com/loftwah/buildkit/settings/secrets/actions).
3. Click “New repository secret.”
4. Name it `SLACK_WEBHOOK_URL`, paste the webhook URL, and click “Add secret.”
5. You’ll get “✅ Done” or “🚨 Failed” messages in Slack.

If you don’t set it up, it runs silently—no interruptions.

## Contributing

- **Issues**: Go to the “Issues” tab and report any problems or ideas.
- **Pull Requests**: Submit changes via the “Pull requests” tab if you’ve got improvements.

## Licensing

This repository only mirrors images. The original licenses for BuildKit, Ruby, Nginx, and Node.js apply to their respective images.

## Status Check

Visit [https://github.com/loftwah/buildkit/actions](https://github.com/loftwah/buildkit/actions):

- Green check = Workflow succeeded.
- Red X = Something failed; click it for detailed logs.
