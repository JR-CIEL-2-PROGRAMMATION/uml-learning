# 07 - Diagrammes de composants & de déploiement

## Diagramme de composants

Montre comment le système est découpé en **modules logiciels** et leurs dépendances.

```mermaid
flowchart LR
    subgraph Frontend
        UI[Interface Web]
    end
    subgraph Backend
        API[API REST]
        Auth[Service Auth]
        Order[Service Commandes]
    end
    subgraph Data
        DB[(Base de données)]
    end
    UI --> API
    API --> Auth
    API --> Order
    Auth --> DB
    Order --> DB
```

Voir aussi : [`exemples/architecture-composants.puml`](./exemples/architecture-composants.puml)

## Diagramme de déploiement

Montre la **répartition physique** des composants sur des serveurs/machines.

```mermaid
flowchart TB
    subgraph "Navigateur Client"
        Browser[Navigateur]
    end
    subgraph "Serveur Web"
        FE[Frontend]
    end
    subgraph "Serveur Application"
        API[API REST]
    end
    subgraph "Serveur BDD"
        DB[(PostgreSQL)]
    end
    Browser -->|HTTPS| FE
    FE -->|API calls| API
    API -->|SQL| DB
```

## Quand les utiliser ?

- **Composants** → architecture logicielle (microservices, modules)
- **Déploiement** → infrastructure (serveurs, cloud, Docker)

## À retenir

- Composants = **logique** (qui dépend de qui)
- Déploiement = **physique** (qui tourne où)
