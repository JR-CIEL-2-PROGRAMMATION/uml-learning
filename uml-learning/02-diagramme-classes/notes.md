# 02 - Diagramme de classes

Le diagramme le plus utilisé en UML. Il décrit la **structure statique** d'un système : les classes, leurs attributs, leurs méthodes, et les relations entre elles.

## Anatomie d'une classe

Une classe se représente par un rectangle à 3 compartiments :

```
┌─────────────────────┐
│      NomClasse       │   ← nom de la classe
├─────────────────────┤
│ - attribut1: Type    │   ← attributs
│ + attribut2: Type    │
├─────────────────────┤
│ + methode1(): Type    │   ← méthodes
│ - methode2(param)     │
└─────────────────────┘
```

### Visibilité

| Symbole | Signification |
|---|---|
| `+` | public |
| `-` | private |
| `#` | protected |
| `~` | package |

## Les relations entre classes

C'est la partie la plus importante à maîtriser.

### 1. Association
Lien simple entre deux classes (« utilise », « connaît »).

```mermaid
classDiagram
    Etudiant --> Cours : suit
```

### 2. Agrégation (losange vide)
Relation "a un", mais les objets peuvent exister indépendamment l'un de l'autre.

```mermaid
classDiagram
    Equipe o-- Joueur
```
*Un joueur peut exister sans équipe.*

### 3. Composition (losange plein)
Relation "a un" forte : si le tout est détruit, les parties le sont aussi.

```mermaid
classDiagram
    Maison *-- Piece
```
*Une pièce n'existe pas sans la maison.*

### 4. Héritage (généralisation)
Relation "est un". Flèche à triangle vide vers la classe mère.

```mermaid
classDiagram
    Animal <|-- Chien
    Animal <|-- Chat
```

### 5. Réalisation / Implémentation
Une classe implémente une interface.

```mermaid
classDiagram
    Volant <|.. Avion
```

### Tableau récapitulatif

| Relation | Symbole Mermaid | Sens |
|---|---|---|
| Association | `-->` | "utilise" |
| Agrégation | `o--` | "a un" (faible) |
| Composition | `*--` | "a un" (forte) |
| Héritage | `<|--` | "est un" |
| Réalisation | `<|..` | "implémente" |

## Exemple complet : système de bibliothèque

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
        +rechercherLivre(String)
    }

    class Emprunt {
        -Date dateEmprunt
        -Date dateRetour
    }

    Bibliotheque "1" *-- "many" Livre : contient
    Membre "1" --> "many" Emprunt : effectue
    Emprunt "1" --> "1" Livre : concerne
```

Le même exemple en PlantUML : voir [`exemples/bibliotheque.puml`](./exemples/bibliotheque.puml)

## Cardinalités (multiplicités)

| Notation | Signification |
|---|---|
| `1` | exactement un |
| `0..1` | zéro ou un |
| `*` ou `0..*` | zéro ou plusieurs |
| `1..*` | un ou plusieurs |
| `n..m` | entre n et m |

## À retenir

- 3 compartiments : nom / attributs / méthodes
- 5 relations clés : association, agrégation, composition, héritage, réalisation
- Bien distinguer agrégation (faible) vs composition (forte) — c'est le piège classique
- Toujours réfléchir aux cardinalités
