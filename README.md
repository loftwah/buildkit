# Docker Image Mirror

[![Mirror Docker Images](https://github.com/loftwah/buildkit/actions/workflows/mirror-images.yml/badge.svg)](https://github.com/loftwah/buildkit/actions/workflows/mirror-images.yml)

This repository (`loftwah/buildkit`) mirrors four Docker images to the GitHub Container Registry (GHCR). Here’s what gets copied:

- `moby/buildkit:latest` → `ghcr.io/loftwah/buildkit/buildkit`
- `ruby:latest` → `ghcr.io/loftwah/buildkit/ruby`
- `nginx:latest` → `ghcr.io/loftwah/buildkit/nginx`
- `node:latest` → `ghcr.io/loftwah/buildkit/nodejs`

It updates these every damn day at midnight UTC (12:00 AM, universal time). You can also run it manually whenever you want.

## Pulling the Images

You need Docker installed (grab it from [docker.com](https://www.docker.com/) if you don’t have it). Open a terminal and use these commands to get the `latest` versions:

```bash
docker pull ghcr.io/loftwah/buildkit/buildkit:latest  # BuildKit
docker pull ghcr.io/loftwah/buildkit/ruby:latest      # Ruby
docker pull ghcr.io/loftwah/buildkit/nginx:latest     # Nginx
docker pull ghcr.io/loftwah/buildkit/nodejs:latest    # Node.js
```

### Specific Versions

The workflow only mirrors `latest` by default, but if upstream has tagged versions (like `ruby:3.2.2` or `node:20`), you can manually trigger those. Examples of what _could_ work if you sync it (see Manual Trigger below):

- `ghcr.io/loftwah/buildkit/buildkit:v0.12.5`
- `ghcr.io/loftwah/buildkit/ruby:3.2.2`
- `ghcr.io/loftwah/buildkit/nginx:1.25.3`
- `ghcr.io/loftwah/buildkit/nodejs:20`

To pull a specific version, swap `latest` with the tag, but it’s only there if you’ve manually synced it first:

```bash
docker pull ghcr.io/loftwah/buildkit/ruby:3.2.2
```

## How to Use These Images

Here’s how to do shit with them after pulling:

### BuildKit

Builds Docker images fast. Run it:

```bash
docker run -d --name buildkitd --privileged ghcr.io/loftwah/buildkit/buildkit:latest
```

Then build something:

```bash
docker buildx create --use --name mybuilder --driver remote tcp://localhost:1234
docker buildx build -t myimage:latest .
```

### Ruby

Run Ruby code. Start it:

```bash
docker run -it ghcr.io/loftwah/buildkit/ruby:latest bash
```

Inside, type:

```bash
ruby -e 'puts "Hell yeah, Ruby works!"'
```

### Nginx

Run a web server. Start it:

```bash
docker run -d -p 80:80 ghcr.io/loftwah/buildkit/nginx:latest
```

Open `http://localhost` in your browser—bam, Nginx.

### Node.js

Run JavaScript. Start it:

```bash
docker run -it ghcr.io/loftwah/buildkit/nodejs:latest bash
```

Inside, type:

```bash
node -e 'console.log("Node.js is alive");'
```

## Automatic Updates

A GitHub Actions workflow handles it:

- Runs every day at midnight UTC.
- You can trigger it manually too.
- Sends Slack messages if you set it up (optional).

## Manual Trigger

To update shit yourself:

1. Go to [https://github.com/loftwah/buildkit/actions](https://github.com/loftwah/buildkit/actions).
2. Click “Mirror Docker Images” in the list.
3. Hit “Run workflow” on the right.
4. **Optional**: Type an image like `moby/buildkit:v0.12.5` or `ruby:3.2.2` to sync just that version. Leave blank for all `latest`.
5. Leave the branch as `main`.
6. Click the green “Run workflow” button. Wait a few minutes.

## What Happens When It Runs

The workflow:

1. Logs into GHCR with a secret token.
2. If you gave it one image (e.g., `ruby:3.2.2`):
   - Pulls it, tags it as `ghcr.io/loftwah/buildkit/ruby`, pushes it.
   - Stops if it screws up.
3. If you didn’t specify anything:
   - Pulls all four `latest` images, tags them, pushes them.
   - Skips any that screw up and keeps going.
4. If Slack’s set up, it pings you.

## Files in This Repo

```
.
├── .github
│   └── workflows
│       └── mirror-images.yml    # The workflow file
└── README.md                    # This doc
```

## Permissions

The workflow needs:

- `packages: write` - To push images.
- `contents: read` - To see the workflow file.

## Slack Setup (Optional)

To get Slack alerts:

1. Make a Slack webhook (search “Slack webhook” online).
2. Go to [https://github.com/loftwah/buildkit/settings/secrets/actions](https://github.com/loftwah/buildkit/settings/secrets/actions).
3. Click “New repository secret.”
4. Name it `SLACK_WEBHOOK_URL`, paste the webhook, hit “Add secret.”
5. It’ll ping Slack with “✅ Done” or “🚨 Failed.”

No webhook? It runs silent.

## Adding or Fixing Stuff

- **Issues**: Click “Issues” on GitHub, complain about what’s wrong.
- **Pull Requests**: Edit files, submit changes if you know how.

## Legal Stuff

This just mirrors images. BuildKit, Ruby, Nginx, and Node.js keep their own licenses.

## Check If It’s Working

Go to [https://github.com/loftwah/buildkit/actions](https://github.com/loftwah/buildkit/actions):

- Green check = It worked.
- Red X = Something’s busted, click it for logs.
