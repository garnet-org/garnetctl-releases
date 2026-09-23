# garnetctl

`garnetctl` is the command line interface for [Garnet](https://garnet.ai) — serving as a interface to the Garnet API, optimized for work in CI, terminals or agentic tool use.

This repository hosts the official binary releases. Linux and macOS builds (x86_64 and arm64) are attached to every [release](https://github.com/garnet-org/garnetctl-releases/releases), along with a `garnetctl_<version>_checksums.txt` for integrity verification.

## Install

```bash
ARCH=$(uname -m); [ "$ARCH" = "aarch64" ] && ARCH=arm64
curl -sL "https://github.com/garnet-org/garnetctl-releases/releases/latest/download/garnetctl_$(uname -s)_${ARCH}.tar.gz" | tar xz garnetctl
sudo install garnetctl /usr/local/bin/

garnetctl version
```

## Quick start

```bash
# Interactive login (device flow — opens a browser approval page)
garnetctl auth login
garnetctl auth whoami

# Machine auth with a project token (no config file needed)
export GARNET_TOKEN=YOUR_PROJECT_TOKEN
garnetctl list profiles --format json

# Inside GitHub Actions (with "id-token: write" permission)
export GARNET_WORKFLOW_TOKEN=$(garnetctl auth exchange-github-oidc --token-only)

# Tokenless: fetch a Runtime Review profile through a shareable link
garnetctl get profile --link-token LINK_TOKEN --format json
```

Authentication precedence: `GARNET_WORKFLOW_TOKEN` > `GARNET_USER_TOKEN` > `GARNET_TOKEN` > stored config token. The API base URL defaults to `https://api.garnet.ai` and can be overridden with `GARNET_BASE_URL`.

Every `get`/`list` command supports `--format table|json|yaml`, making the CLI directly scriptable by agents and CI pipelines.

## Documentation

- Full command reference: run `garnetctl --help`, CLI documentation, and additionally: [docs](https://docs.garnet.ai)

## Notes

- Today, garnetctl is an experimental interface and not recommended for production use. For questions or support, reach out at engineering@garnet.ai or create an issue in this repo. 
