# 05 - Diagramme d'activité

Décrit **un flux d'actions, un algorithme ou un processus métier**, étape par étape, avec des conditions et des parallélismes. C'est le diagramme UML le plus proche d'un **organigramme** classique, mais avec des conventions plus précises.

## Objectifs pédagogiques

- Modéliser un processus métier avec début, actions, décisions et fin
- Utiliser les gardes (`(condition)`) sur les branches de décision
- Représenter le parallélisme avec fork et join
- Utiliser les couloirs (swimlanes) pour distinguer les responsables

---

## 1. Les éléments fondamentaux

| Élément | PlantUML | Rôle |
|---------|----------|------|
| **Début** | `start` | Point d'entrée unique |
| **Fin** | `stop` | Fin du processus |
| **Action** | `:Action;` | Une étape/tâche à accomplir |
| **Décision** | `if (condition) then (Oui)` | Branchement conditionnel |
| **Fork/Join** | `fork` / `end fork` | Parallélisme |
| **Swimlane** | `\|Acteur\|` | Attribue les actions à des responsables |

---

## 2. Exemple simple : traitement d'une commande

```plantuml
@startuml
start
:Recevoir la commande;
if (Stock disponible ?) then (Oui)
    :Préparer la commande;
    fork
        :Générer la facture;
    fork again
        :Réserver le transporteur;
    end fork
    :Expédier la commande;
    :Envoyer email de confirmation;
else (Non)
    :Notifier le client\n(rupture de stock);
endif
stop
@enduml
```

![Diagramme d'activité — Traitement d'une commande](../exemples/traitement-commande.png)

> Les étiquettes `(Oui)` / `(Non)` sur les branches de décision s'appellent des **gardes**.
> Elles doivent couvrir **tous les cas possibles**.

---

## 3. Les gardes (conditions)

Une **garde** est une condition entre parenthèses qui détermine quelle branche est empruntée.

Règles :
- Les gardes doivent être **mutuellement exclusives** (on ne peut pas prendre deux branches en même temps).
- Elles doivent être **exhaustives** (tous les cas sont couverts).

```plantuml
@startuml
start
if (Note >= 10 ?) then (Oui)
    :Admis;
else (Non)
    :Recalé;
endif
stop
@enduml
```

---

## 4. Le parallélisme : Fork et Join

Quand plusieurs actions doivent se dérouler **en même temps**, on utilise :
- **fork** : divise le flux en plusieurs branches parallèles
- **end fork** : attend que **toutes** les branches soient terminées pour continuer

```plantuml
@startuml
start
:Commande validée;
fork
    :Préparer le colis;
fork again
    :Générer la facture;
fork again
    :Réserver le transporteur;
end fork
:Expédier;
stop
@enduml
```

> **Différence Fork vs Décision :**
> - Décision → **un seul chemin** est emprunté (exclusif)
> - Fork → **tous les chemins** sont exécutés en parallèle

---

## 5. Les couloirs (swimlanes)

Les swimlanes permettent de **répartir les actions entre différents acteurs ou services**.

```plantuml
@startuml
|Candidat|
start
:Remplir le formulaire\nd'inscription;

|Système|
if (Places disponibles ?) then (Oui)
    |Candidat|
    :Procéder au paiement;
    |Système|
    if (Paiement réussi ?) then (Oui)
        :Envoyer confirmation;
        :Réserver une place;
    else (Non)
        :Afficher message d'erreur;
    endif
else (Non)
    :Mettre sur liste d'attente;
endif
stop
@enduml
```

![Diagramme d'activité avec swimlanes — Inscription à une formation](../exemples/inscription-formation.png)

---

## 6. Diagramme d'activité vs diagramme de séquence

| | Activité | Séquence |
|-|----------|----------|
| Focus | le **processus/flux** lui-même | les **échanges entre objets** |
| Acteurs visibles ? | optionnel (via swimlanes) | toujours (lignes de vie) |
| Parallélisme | oui (fork/join) | oui (par) |
| Utilisateurs cibles | métier + technique | plutôt technique |
| Bon pour | algorithmes, processus métier | scénarios d'interaction technique |

---

## 7. Méthode pour construire un diagramme d'activité

1. **Définir le début** : qu'est-ce qui déclenche le processus ?
2. **Lister toutes les actions** dans l'ordre.
3. **Identifier les décisions** : à quels endroits le flux peut-il bifurquer ?
4. **Écrire les gardes** sur chaque branche de décision.
5. **Identifier les parallélismes** : des actions peuvent-elles se faire en même temps ?
6. **Identifier la fin** : à quel(s) moment(s) le processus se termine-t-il ?
7. **Ajouter les swimlanes** si plusieurs acteurs sont impliqués.

---

## 8. Erreurs classiques à éviter

| Erreur | Correction |
|--------|------------|
| Gardes incomplètes sur une décision | Toujours couvrir **tous les cas** |
| Confondre fork et décision | Fork = **parallèle** ; Décision = **exclusif** |
| Oublier le end fork après un fork | Chaque fork doit avoir son end fork |
| Trop détailler les actions | Rester à un niveau d'**abstraction cohérent** |
| Plusieurs points de départ | Il n'y a **qu'un seul** état initial |

---

## À retenir

- `start` → actions → `if/else` → `stop`
- Gardes `(condition)` sur les branches — elles doivent tout couvrir
- `fork / end fork` = **parallèle** | `if / else` = **exclusif**
- Swimlanes = "qui fait quoi" (très utile pour les processus multi-acteurs)
- Plus lisible qu'un diagramme de séquence pour un processus métier haut niveau
