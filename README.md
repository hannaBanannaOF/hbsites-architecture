# hbsites-architecture
Architecture and design files for all my sites

### Running stuff
- Common:\
<code>docker-compose -f composes/docker-dev/docker-compose.common-dev.yml -p common up -d</code>

- Auth:\
<code>docker-compose -f composes/docker-dev/docker-compose.auth-dev.yml -p authenticator up -d</code>

- QuestMaster:\
<code>docker-compose -f composes/docker-dev/docker-compose.questmaster-dev.yml -p questmaster up -d</code>