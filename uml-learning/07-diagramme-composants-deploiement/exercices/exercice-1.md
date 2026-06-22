# Exercice 1 — Architecture d'une app de messagerie

## Énoncé

Une application de messagerie instantanée est composée de :

- Une **application mobile** (composant client)
- Un **serveur API** qui gère l'authentification et la messagerie
- Une **base de données** pour stocker les messages
- Un **service de notifications push** séparé, appelé par le serveur API

Côté infrastructure :
- L'application mobile tourne sur le téléphone de l'utilisateur.
- Le serveur API et le service de notifications tournent chacun sur leur propre serveur cloud.
- La base de données tourne sur un serveur dédié.

### Travail à faire

1. Dessine le diagramme de **composants** (dépendances logiques).
2. Dessine le diagramme de **déploiement** (répartition physique).

---

Compare avec [`correction-1.md`](./correction-1.md)
