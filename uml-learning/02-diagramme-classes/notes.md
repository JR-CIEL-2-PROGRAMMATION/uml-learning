# 02 - Diagramme de classes

Le diagramme le plus utilisé en UML. Il décrit la **structure statique** : classes, attributs, méthodes et relations.

## Anatomie d'une classe

```
┌─────────────────────┐
│      NomClasse       │   ← nom
├─────────────────────┤
│ - attribut: Type     │   ← attributs
├─────────────────────┤
│ + methode(): Type    │   ← méthodes
└─────────────────────┘
```

### Visibilité

| Symbole | Signification |
|---|---|
| `+` | public |
| `-` | private |
| `#` | protected |

## Les relations

### Association
```mermaid
classDiagram
    Etudiant --> Cours : suit
```

### Agrégation (losange vide) — "a un" faible
```mermaid
classDiagram
    Equipe o-- Joueur
```

### Composition (losange plein) — "a un" forte
```mermaid
classDiagram
    Maison *-- Piece
```

### Héritage — "est un"
```mermaid
classDiagram
    Animal <|-- Chien
    Animal <|-- Chat
```

### Tableau récapitulatif

| Relation | Mermaid | Sens |
|---|---|---|
| Association | `-->` | "utilise" |
| Agrégation | `o--` | "a un" faible |
| Composition | `*--` | "a un" forte |
| Héritage | `<|--` | "est un" |

## Exemple complet : bibliothèque

```mermaid
classDiagram
    class Livre {
        -String titre
        -String isbn
        -boolean disponible
        +emprunter()
        +retourner()
    }
    class Membre {
        -String nom
        -String idMembre
        +emprunterLivre(Livre)
    }
    class Bibliotheque {
        -List~Livre~ livres
        +ajouterLivre(Livre)
    }
    class Emprunt {
        -Date dateEmprunt
        -Date dateRetour
    }
    Bibliotheque "1" *-- "many" Livre : contient
    Membre "1" --> "many" Emprunt : effectue
    Emprunt "1" --> "1" Livre : concerne
```

Voir aussi : [`exemples/bibliotheque.puml`](./exemples/bibliotheque.puml)

## Cardinalités

| Notation | Signification |
|---|---|
| `1` | exactement un |
| `0..1` | zéro ou un |
| `*` | zéro ou plusieurs |
| `1..*` | un ou plusieurs |

## À retenir

- Bien distinguer **agrégation** (faible) vs **composition** (forte)
- Toujours réfléchir aux cardinalités
