# Running locally

Everything runs with Docker Compose. The compose files live in [`composes/docker-dev/`](https://github.com/hannaBanannaOF/hbsites-architecture/tree/main/composes/docker-dev).

## Prerequisites

- Docker with Docker Compose v2
- The [`gateway`](https://github.com/hannaBanannaOF/gateway) repository cloned **next to** this one. The QuestMaster compose mounts `../../../gateway/config/questmaster-routes.yml` into the gateway container:

```
LiminalLabs/
├── gateway/
└── hbsites-architecture/
```

## 1. Start Auth (Keycloak)

The Auth stack creates the `auth` network that the other stacks join, so it **must be started first**.

```shell
docker compose -f composes/docker-dev/docker-compose.auth-dev.yml -p auth up -d
```

### Import the realm

On the first run, open the admin console at <http://localhost:8080> (`admin` / `password`), create a realm and upload [`realm/LiminalLabs-realm.json`](https://github.com/hannaBanannaOF/hbsites-architecture/blob/main/realm/LiminalLabs-realm.json).

!!! info
    The export already contains the `questmaster` client secret used by the gateway (`LIMINALLABS_GATEWAY_AUTH_KEYCLOAK_CLIENT_SECRET`), but **no users**. Create a test user in the realm before logging in.

## 2. Start QuestMaster

```shell
docker compose -f composes/docker-dev/docker-compose.questmaster-dev.yml -p questmaster up -d
```

Startup order is handled by the compose file: the databases come up first, `questmaster-core-migrate` applies the core migrations and exits, then the core, the gateway and finally the frontend start. The app is at <http://localhost:3000>.

!!! note
    The core loads Keycloak's signing keys on startup, so Keycloak must be running with the `LiminalLabs` realm imported. If it isn't ready yet, the core exits and Docker restarts it until it is.

See [Services & ports](services.md) for what's running where.

## Customizing the environment

Image versions, host ports, public URLs and passwords can be overridden with a `.env` file next to the compose files (it's git-ignored and used by both stacks):

```shell
cp composes/docker-dev/.env.example composes/docker-dev/.env
```

Anything left out of `.env` keeps the default shown in [`.env.example`](https://github.com/hannaBanannaOF/hbsites-architecture/blob/main/composes/docker-dev/.env.example). To keep several setups, pass one explicitly with `--env-file`.

!!! warning
    Database passwords only apply when the volume is first created. After changing one, recreate the volume with `down -v`.

## Stopping

```shell
docker compose -p questmaster down
docker compose -p auth down
```

Add `-v` to also remove the database volumes.

!!! warning
    All credentials in this repository are **for local development only**. Never reuse them in a deployed environment.

## Previewing these docs

```shell
pip install -r requirements-docs.txt
mkdocs serve
```

Then open <http://127.0.0.1:8000>. The site is deployed to GitHub Pages automatically on every push to `main`.
