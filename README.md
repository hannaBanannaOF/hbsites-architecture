# Liminal Labs Architecture
Architecture and design files for all my sites  
*(**Liminal Labs** was just a proxy name I came up, not business related)*

### Running stuff

#### Docker compose
- Auth:
```shell
docker compose -f composes/docker-dev/docker-compose.auth-dev.yml -p auth up -d
```

- QuestMaster:
```shell
docker compose -f composes/docker-dev/docker-compose.questmaster-dev.yml -p questmaster up -d
```