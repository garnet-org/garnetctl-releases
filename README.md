# garnetctl

`garnetctl` is the command line interface for [Garnet](https://garnet.ai) — manage agents, events, issues, network policies, webhooks, and Runtime Review profiles from your terminal or CI.

This repository hosts the official binary releases. Linux and macOS builds (x86_64 and arm64) are attached to every [release](https://github.com/garnet-org/garnetctl-releases/releases), along with a `garnetctl_<version>_checksums.txt` for integrity verification.

## Install

```bash
VERSION=$(curl -sI https://github.com/garnet-org/garnetctl-releases/releases/latest | awk -F'/tag/v' 'tolower($0) ~ /^location:/ {print $2}' | tr -d '\r')
ARCH=$(uname -m); if [ "$ARCH" = "aarch64" ]; then ARCH=arm64; fi
ASSET="garnetctl_$(uname -s)_${ARCH}.tar.gz"

curl -sLO "https://github.com/garnet-org/garnetctl-releases/releases/download/v${VERSION}/${ASSET}"
curl -sLO "https://github.com/garnet-org/garnetctl-releases/releases/download/v${VERSION}/garnetctl_${VERSION}_checksums.txt"
grep "$ASSET" "garnetctl_${VERSION}_checksums.txt" | sha256sum -c -

tar xzf "$ASSET" garnetctl
sudo install garnetctl /usr/local/bin/

garnetctl version
```

On macOS, replace `sha256sum -c -` with `shasum -a 256 -c -`.

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

- Full command reference: run `garnetctl --help` or see the [CLI documentation](https://github.com/garnet-org/control-plane/blob/main/cli/docs/cli.md)
- Garnet platform docs: https://garnet.ai
