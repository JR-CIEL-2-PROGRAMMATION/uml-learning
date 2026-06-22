# Correction — Exercice 1

```mermaid
flowchart TD
    Start([Début]) --> A[Remplir formulaire]
    A --> B{Places disponibles ?}
    B -- Non --> C[Mettre sur liste d'attente]
    B -- Oui --> D[Procéder au paiement]
    D --> E{Paiement réussi ?}
    E -- Non --> F[Afficher erreur]
    F --> D
    E -- Oui --> G[Envoyer confirmation]
    G --> H[Réserver une place]
    C --> End([Fin])
    H --> End
```

## Points clés à vérifier

- ✅ Deux décisions imbriquées (disponibilité, puis paiement) — bien distinguées
- ✅ La boucle de retour en cas d'échec de paiement (le candidat peut retenter)
- ✅ Toutes les branches finissent bien par converger vers la fin
- ✅ "Liste d'attente" est un chemin alternatif complet, pas juste un message
