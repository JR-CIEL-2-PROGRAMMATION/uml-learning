# Exercice 1 — Cycle de vie d'un compte utilisateur

## Énoncé

Modélise les états possibles d'un compte utilisateur sur une plateforme :

- À la création, le compte est **En attente de validation** (email non confirmé).
- Quand l'utilisateur confirme son email, le compte devient **Actif**.
- Un administrateur peut **suspendre** un compte actif → il passe en **Suspendu**.
- Un compte suspendu peut être **réactivé** par un administrateur → retour à **Actif**.
- Un utilisateur peut **supprimer son compte** depuis l'état Actif ou Suspendu → état **Supprimé** (final).
- Si l'email n'est pas confirmé après 7 jours, le compte passe automatiquement en **Expiré** (final).

### Travail à faire

Dessine le diagramme d'états correspondant.

---

La correction est disponible auprès de ton enseignant.
