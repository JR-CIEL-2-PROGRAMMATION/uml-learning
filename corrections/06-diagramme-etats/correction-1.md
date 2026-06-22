# Correction — Exercice 1

```mermaid
stateDiagram-v2
    [*] --> EnAttenteValidation
    EnAttenteValidation --> Actif : email confirmé
    EnAttenteValidation --> Expire : 7 jours sans confirmation
    Actif --> Suspendu : suspension admin
    Suspendu --> Actif : réactivation admin
    Actif --> Supprime : suppression utilisateur
    Suspendu --> Supprime : suppression utilisateur
    Expire --> [*]
    Supprime --> [*]
```

## Points clés à vérifier

- ✅ Deux états finaux distincts (Expiré et Supprimé), tous les deux atteignables depuis des chemins différents
- ✅ La transition automatique ("7 jours sans confirmation") est bien représentée comme un événement (pas une action)
- ✅ Le cycle Actif ↔ Suspendu (aller-retour) est bien représenté avec deux transitions distinctes
- ✅ "Supprimé" est accessible depuis Actif **et** Suspendu (deux flèches entrantes)
