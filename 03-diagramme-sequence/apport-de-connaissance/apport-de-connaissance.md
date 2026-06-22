# 03 - Diagramme de séquence

Décrit **les échanges de messages entre objets ou acteurs dans le temps**. C'est le diagramme idéal pour visualiser ce qui se passe concrètement lorsqu'un scénario précis est déclenché (ex : "que se passe-t-il quand l'utilisateur clique sur 'Payer' ?").

## Objectifs pédagogiques

- Comprendre la notion de ligne de vie (lifeline)
- Distinguer message synchrone, asynchrone et retour
- Utiliser les fragments combinés : `alt`, `opt`, `loop`, `par`
- Savoir modéliser un scénario complet étape par étape

---

## 1. Les éléments fondamentaux

### Participants et lignes de vie

Chaque **participant** (acteur, objet, service…) est représenté en haut du diagramme avec une ligne pointillée qui descend vers le bas. Cette ligne s'appelle la **ligne de vie** (lifeline).

```
Acteur        Objet A       Objet B
  |              |              |
  |              |              |    ← temps s'écoule vers le bas
  |              |              |
```

### Types de messages

| Type | PlantUML | Signification |
|------|----------|---------------|
| Appel synchrone | `A -> B : message` | l'expéditeur attend la réponse |
| Retour de réponse | `B --> A : réponse` | réponse à un appel (en pointillé) |
| Appel asynchrone | `A ->> B : message` | l'expéditeur n'attend pas |

---

## 2. Exemple de base : connexion utilisateur

```plantuml
@startuml
actor Utilisateur
participant Interface
participant Serveur
database BaseDeDonnees

Utilisateur -> Interface : saisit identifiants
Interface -> Serveur : connexion(login, mdp)
Serveur -> BaseDeDonnees : verifierIdentifiants(login, mdp)
BaseDeDonnees --> Serveur : résultat (valide/invalide)

alt identifiants valides
    Serveur --> Interface : token de session
    Interface --> Utilisateur : accès accordé
else identifiants invalides
    Serveur --> Interface : erreur 401
    Interface --> Utilisateur : message d'erreur
end
@enduml
```

![Diagramme de séquence — Connexion utilisateur](../exemples/connexion.png)

**Lecture pas à pas :**
1. L'Utilisateur saisit ses identifiants → l'Interface les reçoit.
2. L'Interface demande au Serveur de vérifier la connexion.
3. Le Serveur interroge la Base de données.
4. La Base répond avec un résultat.
5. Selon le résultat : soit la session est créée, soit une erreur est renvoyée.

---

## 3. Les fragments combinés (blocs de contrôle)

Les fragments permettent de modéliser des **conditions**, des **boucles** et du **parallélisme**.

### 3.1 `alt` / `else` — condition (if / else)

```plantuml
@startuml
participant Client
participant Stock

Client -> Stock : verifierDisponibilite(produit)
alt produit disponible
    Stock --> Client : quantité disponible
else produit épuisé
    Stock --> Client : rupture de stock
end
@enduml
```

### 3.2 `opt` — bloc optionnel (if sans else)

```plantuml
@startuml
participant Client
participant Commande

Client -> Commande : validerCommande()
opt client abonné premium
    Commande --> Client : réduction 10% appliquée
end
Commande --> Client : confirmation commande
@enduml
```

### 3.3 `loop` — répétition

```plantuml
@startuml
participant Utilisateur
participant Panier

loop pour chaque article sélectionné
    Utilisateur -> Panier : ajouterArticle(article)
    Panier --> Utilisateur : article ajouté (sous-total)
end
Utilisateur -> Panier : valider()
@enduml
```

### 3.4 `par` — exécution en parallèle

```plantuml
@startuml
participant Serveur
participant ServiceEmail
participant ServiceSMS

par
    Serveur -> ServiceEmail : envoyerConfirmation(email)
    ServiceEmail --> Serveur : email envoyé
also
    Serveur -> ServiceSMS : envoyerSMS(telephone)
    ServiceSMS --> Serveur : SMS envoyé
end
@enduml
```

### Tableau récapitulatif des fragments

| Fragment | Rôle |
|----------|------|
| `alt / else` | Condition (if / else if / else) |
| `opt` | Bloc optionnel (if sans else) |
| `loop` | Répétition (for, while…) |
| `par / also` | Traitements en parallèle |
| `ref` | Référence à un autre diagramme de séquence |

---

## 4. Les notes

On peut ajouter des **annotations** pour clarifier un message ou un état.

```plantuml
@startuml
participant App
participant API

App -> API : GET /utilisateurs
note right of API : vérifie le token JWT
API --> App : 200 OK (liste utilisateurs)
note over App, API : communication sécurisée HTTPS
@enduml
```

---

## 5. Exemple complet : paiement en ligne

```plantuml
@startuml
actor Client
participant SiteWeb
participant ServicePaiement
database Banque

Client -> SiteWeb : cliquer sur "Payer"
SiteWeb -> ServicePaiement : initierPaiement(montant, carte)
ServicePaiement -> Banque : autoriserTransaction(carte, montant)

alt transaction autorisée
    Banque --> ServicePaiement : autorisation OK
    ServicePaiement --> SiteWeb : paiement accepté
    SiteWeb --> Client : confirmation + numéro commande
else transaction refusée
    Banque --> ServicePaiement : refus
    ServicePaiement --> SiteWeb : paiement refusé
    SiteWeb --> Client : message d'erreur
end
@enduml
```

![Diagramme de séquence — Paiement en ligne](../exemples/paiement.png)

---

## 6. Diagramme de séquence vs diagramme de classes

| | Diagramme de classes | Diagramme de séquence |
|-|----------------------|----------------------|
| Décrit | la **structure** statique | le **comportement** dynamique dans le temps |
| Répond à | "comment c'est organisé ?" | "que se passe-t-il, étape par étape ?" |
| Niveau | conception générale | scénario précis |

> Les deux sont **complémentaires** : les classes définissent qui existe, le diagramme de séquence montre comment ils interagissent.

---

## 7. Méthode pour construire un diagramme de séquence

1. **Identifier le scénario** à modéliser (un et un seul cas d'utilisation à la fois).
2. **Lister les participants** (acteurs, systèmes, services).
3. **Écrire le scénario principal** (cas nominal) de haut en bas.
4. **Ajouter les cas alternatifs** avec `alt` / `opt`.
5. **Ajouter les boucles** si besoin avec `loop`.
6. **Vérifier** : chaque message d'appel a-t-il un retour si nécessaire ?

---

## À retenir

- Le temps s'écoule **de haut en bas**
- Flèches pleines (`->`) = appels | Pointillées (`-->`) = retours
- `alt` = if/else | `opt` = if seul | `loop` = répétition | `par` = parallèle
- Un diagramme de séquence = **un scénario précis**, pas le système entier
- Toujours nommer les messages avec un **verbe d'action**
