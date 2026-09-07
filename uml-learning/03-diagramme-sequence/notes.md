# 03 - Diagramme de séquence

Décrit **les échanges de messages entre objets/acteurs dans le temps**.

## Éléments clés

- **Acteur / Objet** : participant avec une ligne de vie verticale (pointillé)
- **Message** : flèche horizontale
  - flèche pleine `→` : appel synchrone
  - flèche pointillée `-->` : retour de réponse
- **Blocs combinés** : `alt`, `opt`, `loop`

## Exemple : connexion utilisateur

```mermaid
sequenceDiagram
    actor Utilisateur
    participant Interface
    participant Serveur
    participant BaseDeDonnees

    Utilisateur->>Interface: saisit identifiants
    Interface->>Serveur: connexion(login, mdp)
    Serveur->>BaseDeDonnees: vérifierIdentifiants()
    BaseDeDonnees-->>Serveur: résultat

    alt identifiants valides
        Serveur-->>Interface: token de session
        Interface-->>Utilisateur: accès accordé
    else invalides
        Serveur-->>Interface: erreur
        Interface-->>Utilisateur: message d'erreur
    end
```

Voir aussi : [`exemples/connexion.puml`](./exemples/connexion.puml)

## Les blocs combinés

| Bloc | Usage |
|---|---|
| `alt / else` | condition (if / else) |
| `opt` | bloc optionnel |
| `loop` | répétition |

## À retenir

- Le temps s'écoule **de haut en bas**
- Flèches pleines = appels, pointillées = retours
- Utile pour documenter un cas d'utilisation précis
