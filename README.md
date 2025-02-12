# BuildKit Mirror

[![Mirror BuildKit Image](https://github.com/loftwah/buildkit/actions/workflows/mirror.yml/badge.svg)](https://github.com/loftwah/buildkit/actions/workflows/mirror.yml)

This repository maintains a mirror of the official BuildKit image (`moby/buildkit:latest`) in the GitHub Container Registry. The mirror is automatically updated daily at midnight UTC and can also be triggered manually.

## Usage

To pull the mirrored BuildKit image:

```bash
docker pull ghcr.io/loftwah/buildkit/buildkit:latest
```

## Automated Updates

The mirror is automatically updated through a GitHub Actions workflow that:
- Runs daily at midnight UTC
- Can be triggered manually through the GitHub Actions interface

### Manual Trigger

To manually trigger the mirror update:

1. Go to the [Actions tab](https://github.com/loftwah/buildkit/actions)
2. Select the "Mirror BuildKit Image" workflow
3. Click "Run workflow"
4. Select the branch (usually `main`)
5. Click "Run workflow"

## Workflow Details

The mirror process:
1. Authenticates with GitHub Container Registry
2. Pulls the latest BuildKit image from Docker Hub
3. Tags it for GitHub Container Registry
4. Pushes it to `ghcr.io/loftwah/buildkit/buildkit:latest`

## Repository Structure

```
.
├── .github
│   └── workflows
│       └── mirror.yml    # GitHub Actions workflow file
└── README.md            # This file
```

## Permissions

The GitHub Actions workflow requires the following permissions:
- `packages: write` - For pushing to GitHub Container Registry
- `contents: read` - For accessing repository contents

## Contributing

Feel free to open issues or submit pull requests if you find any problems or have suggestions for improvements.

## License

This repository is for mirroring purposes only. BuildKit's original license applies to the mirrored image.

## Status

Check the [Actions tab](https://github.com/loftwah/buildkit/actions) for the current status of the mirror and past runs.
