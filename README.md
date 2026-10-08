# Liminal Labs Architecture

Architecture and design files for all my sites: service diagrams, database models, the shared Keycloak realm and the Docker Compose setup for local development.

> *"Liminal Labs" is just a placeholder name I came up with, it's not a business.*

📖 **Docs: <https://hannabanannaof.github.io/hbsites-architecture/>**

## Repository layout

| Path | What it is |
| --- | --- |
| [`docs/`](docs) | Documentation site (MkDocs Material) |
| [`docs/diagrams/`](docs/diagrams) | draw.io diagrams: services and database models, rendered directly in the docs |
| [`realm/`](realm) | Keycloak realm export (`LiminalLabs`) |
| [`composes/docker-dev/`](composes/docker-dev) | Docker Compose files for local development |

## Quick start

Start Auth first (it creates the shared `auth` network), then QuestMaster:

```shell
docker compose -f composes/docker-dev/docker-compose.auth-dev.yml -p auth up -d
docker compose -f composes/docker-dev/docker-compose.questmaster-dev.yml -p questmaster up -d
```

The QuestMaster stack expects the [`gateway`](https://github.com/hannaBanannaOF/gateway) repo cloned next to this one. See [Running locally](docs/getting-started.md) for the full setup (realm import, ports, credentials).

## Docs locally

```shell
pip install -r requirements-docs.txt
mkdocs serve
```

## License

[GPL-3.0](LICENSE)
