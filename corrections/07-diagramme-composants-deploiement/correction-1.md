# Correction — Exercice 1

## Diagramme de composants

```mermaid
flowchart LR
    Mobile[Application Mobile] --> API[Serveur API]
    API --> DB[(Base de données)]
    API --> Push[Service Notifications Push]
```

## Diagramme de déploiement

```mermaid
flowchart TB
    subgraph "Téléphone utilisateur"
        Mobile[Application Mobile]
    end
    subgraph "Serveur Cloud 1"
        API[Serveur API]
    end
    subgraph "Serveur Cloud 2"
        Push[Service Notifications]
    end
    subgraph "Serveur BDD"
        DB[(Base de données)]
    end

    Mobile -->|HTTPS| API
    API -->|requêtes| Push
    API -->|SQL| DB
```

## Points clés à vérifier

- ✅ Dans le diagramme de composants, on ne précise **pas** où tournent les éléments — seulement leurs dépendances logiques
- ✅ Dans le diagramme de déploiement, chaque composant est bien placé dans un nœud physique distinct (serveur/téléphone)
- ✅ Le service de notifications est appelé par le serveur API, pas directement par le mobile (cohérent avec l'énoncé)
