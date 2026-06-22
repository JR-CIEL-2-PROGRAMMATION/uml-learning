# 07 - Diagrammes de composants & de déploiement

Deux diagrammes structurels qui décrivent l'**architecture technique** d'un système : l'un sous l'angle logique (composants), l'autre sous l'angle physique (déploiement).

## Objectifs pédagogiques

- Comprendre la différence entre vue logique et vue physique
- Modéliser les dépendances entre composants logiciels
- Modéliser la répartition physique sur des nœuds (serveurs, machines)
- Savoir lire une architecture microservices ou en couches

---

## 1. Diagramme de composants

### Définition

Le diagramme de composants montre comment le système est découpé en **modules/composants logiciels** et comment ils **dépendent les uns des autres** via des interfaces.

Un **composant** est une unité logicielle autonome et remplaçable (module, bibliothèque, microservice, API…).

### Éléments clés

| Élément | Rôle |
|---------|------|
| **Composant** | Boîte étiquetée `[NomComposant]` — unité logicielle |
| **Interface fournie** | Ce que le composant **expose** (ex : une API REST) |
| **Interface requise** | Ce dont le composant **a besoin** pour fonctionner |
| **Dépendance** | Flèche pointillée — "utilise" |

### Exemple : architecture d'une application web classique (en couches)

```mermaid
flowchart LR
    subgraph Frontend
        UI[Interface Web\nReact]
    end

    subgraph Backend
        API[API REST\nNode.js]
        Auth[Service\nAuthentification]
        Metier[Service\nMétier]
        Cache[Cache\nRedis]
    end

    subgraph Data
        DB[(Base de données\nPostgreSQL)]
    end

    UI -->|HTTP/JSON| API
    API --> Auth
    API --> Metier
    Auth --> DB
    Metier --> DB
    Metier --> Cache
```

### Exemple : architecture microservices

```mermaid
flowchart TB
    Client([Client Web/Mobile])

    subgraph API_Gateway
        GW[API Gateway]
    end

    subgraph Services
        SvcUser[Service\nUtilisateurs]
        SvcProduct[Service\nProduits]
        SvcOrder[Service\nCommandes]
        SvcNotif[Service\nNotifications]
    end

    subgraph Données
        DBUser[(DB Utilisateurs)]
        DBProduct[(DB Produits)]
        DBOrder[(DB Commandes)]
    end

    subgraph Messaging
        Queue[File de messages\nRabbitMQ]
    end

    Client --> GW
    GW --> SvcUser
    GW --> SvcProduct
    GW --> SvcOrder
    SvcUser --> DBUser
    SvcProduct --> DBProduct
    SvcOrder --> DBOrder
    SvcOrder --> Queue
    Queue --> SvcNotif
```

### Notation PlantUML dédiée (plus fidèle à la norme UML)

```plantuml
@startuml
package "Frontend" {
    [Interface Web]
}

package "Backend" {
    [API REST]
    [Service Authentification]
    [Service Commandes]
}

database "Base de données" as DB

[Interface Web] --> [API REST]
[API REST] --> [Service Authentification]
[API REST] --> [Service Commandes]
[Service Authentification] --> DB
[Service Commandes] --> DB
@enduml
```

---

## 2. Diagramme de déploiement

### Définition

Le diagramme de déploiement montre la **répartition physique** des composants logiciels sur des **nœuds** (serveurs, machines, conteneurs, cloud…). Il répond à la question : "qui tourne où ?"

### Éléments clés

| Élément | Rôle |
|---------|------|
| **Nœud** | Machine physique ou virtuelle (serveur, PC, smartphone…) |
| **Artefact** | Fichier déployé sur un nœud (`.jar`, `.war`, image Docker…) |
| **Association** | Connexion entre nœuds (protocole, réseau) |
| **Composant** | Module logiciel s'exécutant sur un nœud |

### Exemple : application web déployée classiquement

```mermaid
flowchart TB
    subgraph "Poste client"
        Browser[Navigateur Web]
    end

    subgraph "Serveur Web (Nginx)"
        FE[Frontend statique\nHTML/CSS/JS]
    end

    subgraph "Serveur Application (Node.js)"
        API[API REST]
        Auth[Module Auth]
    end

    subgraph "Serveur Base de données"
        DB[(PostgreSQL)]
    end

    subgraph "Service externe"
        Email[Service Email\nSendGrid]
    end

    Browser -->|HTTPS| FE
    FE -->|REST/JSON| API
    API --> Auth
    API -->|SQL| DB
    API -->|SMTP| Email
```

### Exemple : architecture conteneurisée (Docker / Kubernetes)

```mermaid
flowchart TB
    subgraph "Internet"
        Client([Utilisateur])
    end

    subgraph "Cluster Kubernetes"
        subgraph "Pod Frontend"
            FE[Container : React App]
        end
        subgraph "Pod Backend"
            BE[Container : API Node.js]
        end
        subgraph "Pod Cache"
            Redis[Container : Redis]
        end
    end

    subgraph "Cloud Database (RDS)"
        DB[(PostgreSQL managé)]
    end

    Client -->|HTTPS| FE
    FE -->|REST| BE
    BE --> Redis
    BE -->|SQL| DB
```

### Notation PlantUML dédiée

```plantuml
@startuml
node "Serveur Web" {
    artifact "Frontend (React)" as FE
}

node "Serveur Application" {
    artifact "API REST (Node.js)" as API
    artifact "Module Auth" as Auth
}

node "Serveur BDD" {
    database "PostgreSQL" as DB
}

cloud "Service Externe" {
    artifact "SendGrid" as Email
}

FE --> API : HTTPS
API --> Auth
API --> DB : SQL
API --> Email : SMTP
@enduml
```

---

## 3. Composants vs Déploiement : quelle différence ?

| | Composants | Déploiement |
|-|-----------|-------------|
| Niveau | **Logique** | **Physique** |
| Répond à | "Quels modules existent et comment dépendent-ils ?" | "Sur quelles machines tournent-ils ?" |
| Unité | Composant logiciel | Nœud (serveur/machine) |
| Utile pour | Architecture applicative, microservices | Infrastructure, DevOps, mise en production |

---

## 4. Quand les utiliser ?

- **Composants** :
  - Documenter l'architecture d'une application (monolithique ou microservices)
  - Identifier les dépendances entre modules
  - Concevoir les interfaces entre équipes (front, back, données)

- **Déploiement** :
  - Préparer une mise en production
  - Documenter l'infrastructure (pour les équipes Ops/DevOps)
  - Visualiser la répartition sur un cloud (AWS, Azure, GCP)

---

## 5. Méthode pour construire ces diagrammes

### Pour le diagramme de composants
1. **Lister tous les modules** du système (services, bibliothèques, bases de données…).
2. **Identifier les interfaces** : ce que chaque module expose et ce dont il a besoin.
3. **Tracer les dépendances** : qui utilise quoi ?
4. **Regrouper** en packages si plusieurs composants forment une couche logique.

### Pour le diagramme de déploiement
1. **Lister les nœuds physiques** (serveurs, machines, conteneurs).
2. **Placer les composants** sur les nœuds qui les hébergent.
3. **Tracer les connexions réseau** entre nœuds (protocole, port si utile).
4. **Indiquer les artefacts** si nécessaire (fichiers déployés).

---

## 6. Erreurs classiques à éviter

| Erreur | Correction |
|--------|------------|
| Confondre composant et classe | Un composant = **module déployable**, pas une classe |
| Mettre trop de détails techniques | Rester à un niveau d'abstraction **architectural** |
| Oublier les protocoles sur les connexions | Annoter les flèches avec le protocole (HTTPS, SQL, AMQP…) |
| Ne pas distinguer logique et physique | Faire **deux diagrammes** séparés |

---

## À retenir

- **Composants** = vue **logique** (qui dépend de qui dans le code)
- **Déploiement** = vue **physique** (qui tourne où)
- Ces diagrammes sont essentiels pour documenter une architecture système
- PlantUML est plus adapté que Mermaid pour ces deux diagrammes (notation dédiée)
- En entreprise : indispensables dans les dossiers d'architecture technique (DAT)
