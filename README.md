# veNEAR Alpha Unlock Interface

[![Fmt](https://github.com/HackHumanityOrg/alpha-venear-unlock/actions/workflows/fmt.yml/badge.svg?branch=main&event=push)](https://github.com/HackHumanityOrg/alpha-venear-unlock/actions/workflows/fmt.yml)
[![Lint](https://github.com/HackHumanityOrg/alpha-venear-unlock/actions/workflows/lint.yml/badge.svg?branch=main&event=push)](https://github.com/HackHumanityOrg/alpha-venear-unlock/actions/workflows/lint.yml)
[![Tests](https://github.com/HackHumanityOrg/alpha-venear-unlock/actions/workflows/tests.yml/badge.svg?branch=main&event=push)](https://github.com/HackHumanityOrg/alpha-venear-unlock/actions/workflows/tests.yml)
[![Build](https://github.com/HackHumanityOrg/alpha-venear-unlock/actions/workflows/build.yml/badge.svg?branch=main&event=push)](https://github.com/HackHumanityOrg/alpha-venear-unlock/actions/workflows/build.yml)

A Next.js web interface for unlocking NEAR tokens from the veNEAR Alpha contracts (`v.voteagora.near`). Implements the complete 7-step unlock process described in the [official guide](https://github.com/voteagora/agora-near/wiki/How-to:-Unlock-NEAR-in-veNEAR-Alpha-Contracts).

## What This App Does

If you locked NEAR tokens in the House of Stake (HoS) Alpha veNEAR contracts, this app helps you unlock them through a guided interface:

1. **View locked balances** - See your locked, pending, and liquid veNEAR amounts
2. **Monitor staking status** - Check if tokens are staked and manage staking pool operations
3. **Initiate unlock** - Start the 91.25-day (3-month) unlock period
4. **Track progress** - Visual progress bar and countdown timer
5. **Complete unlock** - Finalize after the waiting period
6. **Transfer tokens** - Move NEAR to your main account
7. **Browse all accounts** - Public list of all locked veNEAR accounts

## Prerequisites

- Node.js `20.x`
- pnpm
- Docker Engine + Docker Compose v2 for container deployment

## Quick Start

```bash
pnpm install
pnpm format:check
pnpm lint
pnpm build
```

## Runtime Configuration

The app connects directly to NEAR mainnet RPC and requires no environment variables to run.

## Deployment Workflow

1. Build and start the app with Docker, Docker Compose, or the standalone Node runtime.
2. Verify `http://127.0.0.1:3000/healthz` locally or `https://<your-domain>/healthz` after deployment.

### Docker

Build the production image:

```bash
pnpm docker:build
```

Run it:

```bash
docker run --rm \
  --name alpha-venear-unlock \
  -p 3000:3000 \
  alpha-venear-unlock
```

Smoke check:

```bash
pnpm health
```

The image uses Next.js standalone output, runs as a non-root user, and honors `PORT`/`HOSTNAME` at runtime.

### Docker Compose

Start the stack:

```bash
pnpm docker:up
```

Inspect and verify:

```bash
docker compose ps
docker compose logs -f web
pnpm health
```

Stop it:

```bash
pnpm docker:down
```

### Standalone Node Runtime

If you want to run the standalone server without Docker:

```bash
pnpm build
HOSTNAME=0.0.0.0 PORT=3000 node .next/standalone/server.js
```

## Local Development

Run the app in dev mode:

```bash
pnpm dev
```

Open `http://localhost:3000`.

Production-like local run:

```bash
pnpm build
pnpm start
```

## Quality Gates

```bash
pnpm format:check
pnpm lint
pnpm build
pnpm test:simple
```

## Testing

Contract tests run against NEAR mainnet RPC to verify lockup account state:

```bash
pnpm test:simple              # Non-staking accounts
pnpm test:staking              # Accounts with staking pools
pnpm test:contract             # All accounts
pnpm test:all                  # All accounts with report output
```

## GitHub Workflows

| Workflow | Purpose                                                                           |
| -------- | --------------------------------------------------------------------------------- |
| `Fmt`    | Runs `pnpm format:check`                                                          |
| `Lint`   | Runs `pnpm lint`                                                                  |
| `Tests`  | Runs `pnpm test:simple` (non-staking contract tests)                              |
| `Build`  | Builds the app, validates `docker-compose.yml`, and smoke-builds the Docker image |

## References

- [How to Unlock NEAR in veNEAR Alpha Contracts](https://github.com/voteagora/agora-near/wiki/How-to:-Unlock-NEAR-in-veNEAR-Alpha-Contracts) - Official unlock guide
- [House of Stake Contracts](https://github.com/houseofstake/house-of-stake-contracts) - Contract source code
- [v.voteagora.near](https://nearblocks.io/address/v.voteagora.near) - veNEAR contract on NEARBlocks

## Artifact Index

- Root deployment guide: `README.md`
- Docker ignore: `.dockerignore`
- Compose stack: `docker-compose.yml`
- Dockerfile: `Dockerfile`
- Health endpoint: `src/app/healthz/route.ts`
- Workflow files: `.github/workflows/fmt.yml`, `.github/workflows/lint.yml`, `.github/workflows/tests.yml`, `.github/workflows/build.yml`
