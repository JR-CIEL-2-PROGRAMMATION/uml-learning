# 05 - Diagramme d'activité

Décrit **un flux/algorithme/processus métier**, étape par étape.

## Éléments clés

| Élément | Représentation |
|---|---|
| Début | rond noir plein |
| Fin | rond noir cerclé |
| Action | rectangle arrondi |
| Décision | losange |
| Fork/Join | barre noire (parallélisme) |

## Exemple : traitement d'une commande

```mermaid
flowchart TD
    Start([Début]) --> A[Recevoir la commande]
    A --> B{Stock disponible ?}
    B -- Oui --> C[Préparer la commande]
    B -- Non --> D[Notifier rupture de stock]
    C --> E[Expédier]
    E --> F[Envoyer confirmation]
    D --> End([Fin])
    F --> End
```

Voir aussi : [`exemples/traitement-commande.puml`](./exemples/traitement-commande.puml)

## Exemple avec parallélisme

```mermaid
flowchart TD
    Start([Début]) --> A[Commande validée]
    A --> B[Préparer colis]
    A --> C[Générer facture]
    B --> D[Expédier]
    C --> D
    D --> End([Fin])
```

## À retenir

- Très lisible pour des non-techniques
- Le losange = toujours une décision (conditions sur les flèches)
- Utilise des **couloirs (swimlanes)** si plusieurs acteurs sont impliqués
