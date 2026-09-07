# Exercice 1 — Cycle de vie d'un compte utilisateur

## Énoncé

Modélise les états possibles d'un compte utilisateur :

- À la création, le compte est **En attente de validation**.
- Quand l'utilisateur confirme son email → compte **Actif**.
- Un admin peut **suspendre** un compte actif → **Suspendu**.
- Un compte suspendu peut être **réactivé** → retour à **Actif**.
- Un utilisateur peut **supprimer son compte** (depuis Actif ou Suspendu) → **Supprimé** (final).
- Sans confirmation après 7 jours → **Expiré** (final).

### Travail à faire

Dessine le diagramme d'états correspondant.
