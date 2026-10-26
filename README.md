# Growbud Edge Server

Run Growbud on your own computer. The Edge Server runs with Docker Compose
on Linux, macOS or Windows. It can work offline, or link to one Growbud
Cloud organization.

This repository publishes the releases. It does not hold the source code.

## Install

Install Docker first. Then run this on Linux or macOS:

```bash
curl -fsSL https://growbud.io/edge/install.sh | sudo bash
```

Run the same command again to upgrade. The installer keeps your settings
and backs up both databases first.

For Windows, an offline install, and every other step, see
https://growbud.io/edge/download and the `README.md` in the bundle.

## Releases

Each [release](https://github.com/growbud-io/edge/releases) holds:

| File | What it is |
| --- | --- |
| `growbud-edge-<version>.tar.gz` | The install bundle |
| `growbud-edge-<version>.tar.gz.sha256` | Its SHA-256 checksum |
| `growbud-edge-<version>.tar.gz.cosign.bundle` | Its signature |
| `install.sh` | The installer that `growbud.io/edge/install.sh` serves |
| `growbud-edge-images-<version>.txt` | The images, pinned by digest |

The images are at `ghcr.io/growbud-io/edge/<name>:<version>`, for
linux/amd64 and linux/arm64. You do not need an account to pull them.

## Verify a download

Install [Cosign](https://docs.sigstore.dev/cosign/installation/).

Verify the bundle before you extract it:

```bash
cosign verify-blob \
  --bundle growbud-edge-<version>.tar.gz.cosign.bundle \
  --certificate-identity-regexp '^https://github.com/growbud-io/edge/\.github/workflows/release\.yml@refs/heads/main$' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  growbud-edge-<version>.tar.gz
```

Verify an image. Use the digest from `growbud-edge-images-<version>.txt`:

```bash
cosign verify \
  --certificate-identity-regexp '^https://github.com/growbud-io/edge/\.github/workflows/release\.yml@refs/heads/main$' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  ghcr.io/growbud-io/edge/growbud-web@sha256:<digest>
```

The signatures are keyless. They prove that the release workflow in this
repository published the file. If cosign shows an error, do not use it.

## Licence

The software is free to use under the [EULA](EULA.md). The other images
the Edge Server runs (Supabase, Kong, Valkey, TimescaleDB, Mosquitto and
ChirpStack) come from their publishers, under their own licences.

## Security

Report a vulnerability to support@growbud.io. Do not open a public issue.
