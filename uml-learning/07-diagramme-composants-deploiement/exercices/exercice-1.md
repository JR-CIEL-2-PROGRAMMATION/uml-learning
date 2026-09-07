# Exercice 1 — Architecture d'une app de messagerie

## Énoncé

Une application de messagerie est composée de :

- Une **application mobile** (client)
- Un **serveur API** (authentification + messagerie)
- Une **base de données** pour stocker les messages
- Un **service de notifications push** séparé, appelé par le serveur API

Infrastructure :
- L'app mobile tourne sur le téléphone de l'utilisateur
- Le serveur API et le service de notifications ont chacun leur propre serveur cloud
- La base de données tourne sur un serveur dédié

### Travail à faire

1. Dessine le diagramme de **composants** (dépendances logiques).
2. Dessine le diagramme de **déploiement** (répartition physique).
