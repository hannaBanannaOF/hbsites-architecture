# QuestMaster database

Each service owns its own database. Game system modules reference core entities through `core_*` columns (e.g. `core_character_sheet_id`, `core_campaing_id`) instead of foreign keys across databases.

## Core

```mermaid
erDiagram
    campaign {
        bigint id PK
        varchar(255) name "NOT NULL"
        uuid dm_id "NOT NULL, indexed"
        varchar(50) dm_name "NOT NULL"
        varchar(45) status "NOT NULL, default DRAFT"
        varchar(45) game_system "NOT NULL"
        varchar(255) slug UK "gerado por trigger"
        text overview
    }

    character_sheet {
        bigint id PK
        varchar(255) name "NOT NULL"
        uuid player_id "nullable, indexed (null = premade)"
        varchar(50) player_name "nullable"
        bigint campaign_id FK "nullable, ON DELETE SET NULL"
        varchar(45) game_system "NOT NULL"
        varchar(255) slug UK "gerado por trigger, refeito ao renomear"
    }

    campaign_invite {
        bigint id PK
        bigint campaign_id FK,UK "NOT NULL, ON DELETE CASCADE"
        uuid hash UK "default gen_random_uuid()"
    }

    campaign_access {
        uuid user_id PK
        bigint campaign_id PK,FK "ON DELETE CASCADE"
        timestamptz accessed_at "default now()"
    }

    character_access {
        uuid user_id PK
        bigint character_id PK,FK "ON DELETE CASCADE"
        timestamptz accessed_at "default now()"
    }

    campaign ||--o{ character_sheet : "tem"
    campaign ||--o| campaign_invite : "tem um convite"
    campaign ||--o{ campaign_access : "acessada por"
    character_sheet ||--o{ character_access : "acessada por"
```

## Call of Cthulhu

![QuestMaster - Call of Cthulhu](../diagrams/databasemodel.drawio)

## Ghostbusters

![QuestMaster - Ghostbusters](../diagrams/databasemodel.drawio)
