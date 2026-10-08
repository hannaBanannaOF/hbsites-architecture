# QuestMaster

Tabletop RPG platform: campaigns, character sheets and campaign invites, with game system specific modules (Call of Cthulhu, Ghostbusters).

![QuestMaster](../diagrams/microservices.drawio)

## Services

| Service | Stack | Status | Description |
| --- | --- | --- | --- |
| `questmaster-frontend` | Next.js | ✅ | Web app; calls the API through the gateway |
| `questmaster-app` | Flutter | 🗓️ Planned | Mobile app |
| `questmaster-core` | Go + Gin, Postgres, S3 | ✅ | Campaigns, character sheets, invites, user profile |
| `questmaster-coc` | Go, Postgres | 🗓️ Planned | Call of Cthulhu rules and character data |

### questmaster-core

- **Campaigns**: create, list, update lifecycle (`DRAFT → ACTIVE → PAUSED/ARCHIVED`), delete. Slugs are generated automatically.
- **Character sheets**: create, list, update current HP, attach/detach from campaigns.
- **Invites**: UUID invite links; players accept by binding a character sheet.
- **API docs**: Swagger UI at `/swagger/index.html` (<http://localhost:8082/swagger/index.html> locally).

| Variable | Description |
| --- | --- |
| `RUN_ADDR` | HTTP listen address (default `0.0.0.0:8080`) |
| `OIDC_HOST` | Keycloak realm URL used to fetch the JWKS |
| `DB_URL` | Postgres connection string |

See also: [Database models](database.md).
