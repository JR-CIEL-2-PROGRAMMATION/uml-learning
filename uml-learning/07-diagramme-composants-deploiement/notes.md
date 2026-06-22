# 07 - Diagrammes de composants & de déploiement

Deux diagrammes structurels moins fréquents au quotidien, mais utiles pour documenter l'architecture technique d'un système.

## Diagramme de composants

Montre comment le système est découpé en **modules/composants logiciels** et leurs dépendances (via des interfaces).

```mermaid
flowchart LR
    subgraph Frontend
        UI[Interface Web]
    end
    subgraph Backend
        API[API REST]
        Auth[Service Authentification]
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

### Notation PlantUML dédiée

```plantuml
@startuml
[Interface Web] --> [API REST]
[API REST] --> [Service Authentification]
[API REST] --> [Service Commandes]
[Service Authentification] --> [Base de données]
[Service Commandes] --> [Base de données]
@enduml
```

## Diagramme de déploiement

Montre la **répartition physique** des composants logiciels sur des machines/serveurs/conteneurs.

```mermaid
flowchart TB
    subgraph "Serveur Web (Nginx)"
        FE[Frontend statique]
    end
    subgraph "Serveur Application"
        API[API REST - Node.js]
    end
    subgraph "Serveur Base de données"
        DB[(PostgreSQL)]
    end
    subgraph "Navigateur Client"
        Browser[Navigateur]
    end

    Browser -->|HTTPS| FE
    FE -->|API calls| API
    API -->|SQL| DB
```

### Notation PlantUML dédiée (avec nœuds physiques)

```plantuml
@startuml
node "Serveur Web" {
  [Frontend]
}
node "Serveur Application" {
  [API REST]
}
node "Serveur BDD" {
  database "PostgreSQL"
}

[Frontend] --> [API REST] : HTTPS
[API REST] --> PostgreSQL : SQL
@enduml
```

## Quand les utiliser ?

- **Composants** : pour documenter une architecture logicielle (microservices, modules) — utile dans une doc d'architecture technique
- **Déploiement** : pour documenter l'infrastructure (serveurs, cloud, conteneurs Docker/Kubernetes) — utile pour les équipes DevOps/Ops

## À retenir

- Composants = **logique** (qui dépend de qui dans le code)
- Déploiement = **physique** (qui tourne où)
- Moins utilisés que classes/séquence au quotidien, mais essentiels pour documenter une architecture système complète
