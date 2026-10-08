# Services & ports

## Containers

| Service | Container | Host port | Notes |
| --- | --- | --- | --- |
| Keycloak | `keycloak` | `8080` | Admin: `admin` / `password` |
| Keycloak DB (Postgres 16) | `keycloak-db` | `5432` | `postgres` / `postgres`, db `keycloak` |
| Gateway | `questmaster-gateway` | `8081` | Health: `/actuator/health` · OAuth callback: `/oauth/callback` |
| Gateway DB (MongoDB 7) | `questmaster-gateway-db` | `27018` | `root` / `mongodb`, db `gateway` |
| QuestMaster Core | `questmaster-core` | `8082` | Swagger: `/swagger/index.html` |
| QuestMaster Core migrations | `questmaster-core-migrate` | — | One-off: applies the migrations and exits; the core only starts if it succeeds |
| QuestMaster Core DB (Postgres 16) | `questmaster-core-db` | `5433` | `postgres` / `postgres`, db `questmaster-core` |
| QuestMaster Frontend | `questmaster-frontend` | `3000` | Talks to the backend through the gateway. Must match the gateway's `ALLOWED_ORIGIN` / `FRONTEND_URL` |

Images are published at `ghcr.io/hannabanannaof/...`.

## Gateway routes

Defined in `gateway/config/questmaster-routes.yml`:

| Path | Upstream |
| --- | --- |
| `/core/api/**` | `questmaster-core:8080` |
| `/coc/api/**` | `questmaster-coc:8080` |

## Docker networks

| Network | Used by |
| --- | --- |
| `auth` | Keycloak, gateway, backends (anything that needs to reach Keycloak) |
| `auth_db` | Keycloak ↔ its database |
| `questmaster` | Frontend, gateway, backends |
| `questmaster_db` | QuestMaster services ↔ their databases |

The `auth` network is created by the Auth stack and declared as `external` by the others, which is why Auth has to be started first.
