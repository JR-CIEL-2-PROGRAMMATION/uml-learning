# 06 - Diagramme d'états (state machine)

Décrit **le cycle de vie d'un objet** : tous les états qu'il peut prendre, et les **transitions** (événements ou conditions) qui le font passer d'un état à un autre. On parle aussi de "machine à états" ou "automate".

## Objectifs pédagogiques

- Identifier les états possibles d'un objet
- Modéliser les transitions et leurs déclencheurs
- Utiliser les actions `entry`, `exit`, `do`
- Représenter des états composites (avec sous-états)

---

## 1. Différence avec le diagramme d'activité

| | Diagramme d'états | Diagramme d'activité |
|-|-------------------|----------------------|
| Centré sur | **un objet** et ses états possibles | **un processus** global |
| Question | "Dans quel état est cet objet à tout moment ?" | "Quelles sont les étapes du processus ?" |
| Idéal pour | Modéliser la vie d'une entité (commande, compte…) | Modéliser un algorithme ou workflow |

---

## 2. Les éléments fondamentaux

| Élément | Représentation | Rôle |
|---------|----------------|------|
| **État initial** | Rond noir plein ● | Point d'entrée — l'objet commence ici |
| **État** | Rectangle arrondi | Situation stable de l'objet |
| **État final** | Rond cerclé ⊙ | L'objet a terminé son cycle de vie |
| **Transition** | Flèche | Passage d'un état à un autre |
| **Événement** | Label sur la flèche | Ce qui déclenche la transition |
| **Garde** | `[condition]` sur la flèche | Condition qui doit être vraie pour franchir la transition |
| **Action** | `/action` sur la flèche | Action déclenchée au moment de la transition |

---

## 3. Anatomie d'une transition

Une transition peut être annotée de trois façons :

```
événement [garde] / action
```

- **événement** : ce qui se passe (ex : `paiementReçu`)
- **[garde]** : condition vérifiée avant de prendre la transition (ex : `[montant > 0]`)
- **/action** : ce qui est exécuté au moment de franchir la transition (ex : `/confirmerCommande()`)

Tous ces éléments sont optionnels, mais au moins un doit être présent.

**Exemple :**
```
EnAttente --> Confirmée : paiementReçu [montant > 0] / envoyerEmail()
```
*Lecture : depuis l'état EnAttente, si l'événement paiementReçu arrive ET que le montant est positif, alors on passe à l'état Confirmée et on exécute envoyerEmail().*

---

## 4. Les actions dans un état

Un état peut avoir des **actions internes** :

| Mot-clé | Déclenchement |
|---------|---------------|
| `entry /` | À l'**entrée** dans l'état |
| `do /` | **Pendant** tout le temps où on est dans l'état (continu) |
| `exit /` | À la **sortie** de l'état |

```mermaid
stateDiagram-v2
    state "EnPréparation" as prep {
        entry: démarrer le chronomètre
        do: préparer les articles
        exit: arrêter le chronomètre
    }
```

---

## 5. Exemple : cycle de vie d'une commande

```mermaid
stateDiagram-v2
    [*] --> EnAttente : commande créée

    EnAttente --> Confirmée : paiement reçu
    EnAttente --> Annulée : annulation client
    EnAttente --> Expirée : délai 30 min dépassé

    Confirmée --> EnPréparation : stock validé
    Confirmée --> Annulée : annulation client

    EnPréparation --> Expédiée : colis remis au transporteur
    Expédiée --> Livrée : réception confirmée par client
    Expédiée --> EnLitige : problème signalé

    Annulée --> [*]
    Expirée --> [*]
    Livrée --> [*]
    EnLitige --> [*]
```

---

## 6. Exemple : cycle de vie d'un compte utilisateur

```mermaid
stateDiagram-v2
    [*] --> EnAttenteValidation : inscription soumise

    EnAttenteValidation --> Actif : email confirmé
    EnAttenteValidation --> Expiré : délai 7 jours dépassé / supprimerDonnées()

    Actif --> Suspendu : suspension admin [motif signalé]
    Suspendu --> Actif : réactivation admin
    Actif --> Supprimé : suppression par l'utilisateur
    Suspendu --> Supprimé : suppression par l'utilisateur

    Expiré --> [*]
    Supprimé --> [*]
```

---

## 7. Les états composites (sous-états)

Un état peut **contenir d'autres états** pour décomposer un comportement complexe. Cela permet de ne pas surcharger le diagramme principal.

```mermaid
stateDiagram-v2
    [*] --> EnPréparation

    state EnPréparation {
        [*] --> Emballage
        Emballage --> ControleQualité : emballage terminé
        ControleQualité --> Emballage : non conforme
        ControleQualité --> [*] : conforme
    }

    EnPréparation --> Expédiée : colis validé
    Expédiée --> [*]
```

*L'état EnPréparation est lui-même une mini machine à états.*

---

## 8. Méthode pour construire un diagramme d'états

1. **Choisir l'objet** à modéliser (une entité dont l'état change dans le temps).
2. **Lister tous les états possibles** : dans quelles situations stables peut-il se trouver ?
3. **Identifier l'état initial** et le(s) état(s) final(s).
4. **Pour chaque paire d'états**, demander : "qu'est-ce qui peut faire passer de l'un à l'autre ?" → c'est l'événement.
5. **Ajouter les gardes** si la transition n'est possible que sous certaines conditions.
6. **Vérifier** que tous les états ont au moins une transition entrante (sauf l'état initial) et une sortante (sauf l'état final).

---

## 9. Exemples d'objets typiquement modélisés avec ce diagramme

| Objet | États typiques |
|-------|----------------|
| Commande | En attente → Confirmée → En préparation → Expédiée → Livrée |
| Compte utilisateur | En attente → Actif → Suspendu → Supprimé |
| Ticket de support | Ouvert → En cours → Résolu → Fermé |
| Connexion réseau | Déconnecté → Connexion en cours → Connecté → Erreur |
| Feu de circulation | Rouge → Vert → Orange → Rouge |

---

## 10. Erreurs classiques à éviter

| Erreur | Correction |
|--------|------------|
| Confondre état et action | Un état = situation **stable** ; une action = événement **ponctuel** |
| Oublier l'état initial | Toujours commencer par ● |
| États sans transition de sortie | Tout état doit pouvoir en sortir (sauf l'état final) |
| Trop d'états | Regrouper les états similaires ou utiliser des sous-états |
| Nommer les états avec des verbes | Les états se nomment avec des **participes passés ou noms** : "Confirmée", "EnAttente" |

---

## À retenir

- Un diagramme d'états = cycle de vie d'**un seul objet**
- Transition = `événement [garde] / action` (tout est optionnel sauf le déclencheur)
- `entry / exit / do` permettent d'attacher des actions à un état lui-même
- Les états composites permettent de décomposer des états complexes
- Très utile pour : commandes, comptes, tickets, connexions réseau…
