# ScrumFlow — Diagrammes de Cas d'Utilisation
## Contexte : 📅 Project & Sprint Planning *(Core Domain)*

> Ce document présente l'ensemble des diagrammes UML de cas d'utilisation du **Core Domain** `Project & Sprint Planning` de l'application ScrumFlow, modélisés avec la syntaxe **Mermaid**.

---

## 🎭 Acteurs impliqués dans ce contexte

| Acteur | Rôle |
|---|---|
| **Product Owner (PO)** | Priorise le backlog, crée les epics/stories, pilote le projet |
| **Scrum Master (SM)** | Facilite les cérémonies Scrum, planifie et clôture les sprints |
| **Développeur** | Estime les stories, consulte le tableau Kanban |
| **Administrateur** | Peut créer des projets aux côtés du PO |
| **Système** | Acteur secondaire automatique (calculs, événements de domaine) |

---

## 📌 UC-4.1 — Créer un Projet

```mermaid
---
title: UC-4.1 — Créer un Projet
---
flowchart LR
    PO(["👤 Product Owner"])
    Admin(["👤 Administrateur"])
    SYS(["⚙️ Système"])

    UC1["(Saisir les informations du projet\nnom · description · dates · équipe)"]
    UC2["(Associer un budget initial)"]
    UC3["(Associer des parties prenantes)"]
    UC4["(Créer l'agrégat Projet)"]
    UC5["(Valider la cohérence des dates)"]

    PO --> UC1
    Admin --> UC1
    UC1 --> UC2
    UC2 --> UC3
    UC3 --> UC4
    SYS --> UC4
    SYS --> UC5
    UC5 -.->|"«extend»"| UC1
```

> **Préconditions** : L'acteur est authentifié avec le rôle PO ou Admin.  
> **Post-conditions** : L'agrégat `Projet` est crée à l'état *"Initialisé"*.

---

## 📌 UC-4.2 — Créer un Epic

```mermaid
---
title: UC-4.2 — Créer un Epic
---
flowchart LR
    PO(["👤 Product Owner"])

    UC1["(Sélectionner un Projet)"]
    UC2["(Définir le titre de l'Epic)"]
    UC3["(Rédiger la description)"]
    UC4["(Définir la valeur métier)"]
    UC5["(Enregistrer l'Epic)"]
    UC6["(Rattacher à un Projet existant)"]

    PO --> UC1
    UC1 --> UC2
    UC2 --> UC3
    UC3 --> UC4
    UC4 --> UC5
    UC6 -.->|"«include»"| UC5
```

> **Préconditions** : Un `Projet` existe et est à l'état actif.  
> **Post-conditions** : L'`Epic` est créé et rattaché au projet.

---

## 📌 UC-4.3 — Créer une User Story

```mermaid
---
title: UC-4.3 — Créer une User Story
---
flowchart LR
    PO(["👤 Product Owner"])
    SYS(["⚙️ Système"])

    UC1["(Rédiger la User Story\n« En tant que… je veux… afin de… »)"]
    UC2["(Définir les critères d'acceptation)"]
    UC3["(Rattacher à un Epic)"]
    UC4["(Valider le format de la story)"]
    UC5["(Ajouter au Product Backlog)"]

    PO --> UC1
    UC1 --> UC2
    UC2 --> UC5
    UC3 -.->|"«extend»"| UC5
    SYS --> UC4
    UC4 -.->|"«include»"| UC1
```

> **Préconditions** : Un `Projet` existe ; un `Epic` peut exister (optionnel).  
> **Post-conditions** : La `User Story` est créée dans le Product Backlog à l'état *"Non estimée"*.

---

## 📌 UC-4.4 — Estimer une User Story (Story Points)

```mermaid
---
title: UC-4.4 — Estimer une User Story (Story Points)
---
flowchart LR
    DEV(["👤 Développeur"])
    SM(["👤 Scrum Master"])
    SYS(["⚙️ Système"])

    UC1["(Sélectionner une User Story)"]
    UC2["(Animer le Planning Poker)"]
    UC3["(Proposer une estimation\nen Story Points)"]
    UC4["(Discuter et converger\nvers un consensus)"]
    UC5["(Enregistrer les Story Points)"]
    UC6["(Mettre à jour l'état\nde la story → « Estimée »)"]

    DEV --> UC1
    SM --> UC2
    UC1 --> UC3
    UC2 -.->|"«include»"| UC3
    UC3 --> UC4
    UC4 --> UC5
    SYS --> UC6
    UC5 --> UC6
```

> **Acteur principal** : Équipe de développement.  
> **Acteur secondaire** : Scrum Master (facilitateur).  
> **Post-conditions** : La story possède une valeur en Story Points, elle change d'état vers *"Estimée"*.

---

## 📌 UC-4.5 — Planifier un Sprint (Sprint Planning)

```mermaid
---
title: UC-4.5 — Planifier un Sprint (Sprint Planning)
---
flowchart LR
    SM(["👤 Scrum Master"])
    PO(["👤 Product Owner"])
    DEV(["👤 Développeur"])
    SYS(["⚙️ Système"])

    UC1["(Créer un Sprint\ndates début/fin + Sprint Goal)"]
    UC2["(Consulter la capacité\nde l'équipe)"]
    UC3["(Sélectionner les User Stories\ndu backlog)"]
    UC4["(Vérifier que la charge\nne dépasse pas la capacité)"]
    UC5["(Constituer le Sprint Backlog)"]
    UC6["(Activer le Sprint)"]
    UC7["(Définir le Sprint Goal)"]

    SM --> UC1
    PO --> UC1
    DEV --> UC3
    UC1 --> UC2
    UC2 --> UC3
    UC7 -.->|"«include»"| UC1
    UC3 --> UC4
    SYS --> UC4
    UC4 --> UC5
    UC5 --> UC6
    SYS --> UC6
```

> **Préconditions** : Le Product Backlog est priorisé et les stories sont estimées.  
> **Post-conditions** : Le `Sprint` passe à l'état *"Actif"* ; le Sprint Backlog est constitué.

---

## 📌 UC-4.6 — Définir le Sprint Goal

```mermaid
---
title: UC-4.6 — Définir le Sprint Goal
---
flowchart LR
    PO(["👤 Product Owner"])
    SM(["👤 Scrum Master"])
    DEV(["👤 Développeur"])

    UC1["(Formuler l'objectif du Sprint\n— Sprint Goal —)"]
    UC2["(Valider collectivement\navec l'équipe)"]
    UC3["(Associer le Sprint Goal\nau Sprint)"]

    PO --> UC1
    UC1 --> UC2
    SM --> UC2
    DEV --> UC2
    UC2 --> UC3
```

> **Post-conditions** : Le `Sprint Goal` est enregistré et visible par tous les membres de l'équipe.

---

## 📌 UC-4.7 — Clôturer un Sprint (Sprint Review + Retrospective)

```mermaid
---
title: UC-4.7 — Clôturer un Sprint
---
flowchart LR
    SM(["👤 Scrum Master"])
    SYS(["⚙️ Système"])

    UC1["(Déclencher la clôture du Sprint)"]
    UC2["(Calculer les stories\nTerminées / Non terminées)"]
    UC3["(Proposer la réintégration\ndes stories non terminées\nau Product Backlog)"]
    UC4["(Passer le Sprint → « Terminé »)"]
    UC5["(Émettre l'événement\nde domaine SprintClos)"]
    UC6["(Générer le rapport de sprint)"]

    SM --> UC1
    UC1 --> UC2
    SYS --> UC2
    UC2 --> UC3
    UC3 --> UC4
    UC4 --> UC5
    SYS --> UC5
    UC5 --> UC6
    SYS --> UC6
```

> **Post-conditions** : Le sprint est *"Terminé"* ; l'événement `SprintClos` est émis vers **Monitoring & Reporting** et **Scrum Artifacts**.

---

## 📌 UC-4.8 — Modifier le périmètre d'un Sprint en cours

```mermaid
---
title: UC-4.8 — Modifier le périmètre d'un Sprint en cours
---
flowchart LR
    PO(["👤 Product Owner"])
    SM(["👤 Scrum Master"])
    SYS(["⚙️ Système"])

    UC1["(Sélectionner le Sprint actif)"]
    UC2["(Ajouter ou retirer\nune User Story)"]
    UC3["(Saisir une justification)"]
    UC4["(Vérifier la capacité\nde l'équipe après modification)"]
    UC5["(Valider la modification)"]
    UC6["(Alerter si surcharge\nde capacité dépassée)"]

    PO --> UC1
    UC1 --> UC2
    UC2 --> UC3
    SM --> UC3
    UC3 --> UC4
    SYS --> UC4
    UC4 --> UC5
    UC6 -.->|"«extend»"| UC4
    SYS --> UC6
```

> **Extensions** : Alerte si la modification entraîne un dépassement de capacité.

---

## 📌 UC-4.9 — Consulter le tableau Kanban du Sprint

```mermaid
---
title: UC-4.9 — Consulter le tableau Kanban du Sprint
---
flowchart LR
    DEV(["👤 Développeur"])
    SM(["👤 Scrum Master"])
    PO(["👤 Product Owner"])

    UC1["(Accéder au Sprint courant)"]
    UC2["(Afficher les colonnes Kanban\nÀ faire | En cours | En revue | Terminé)"]
    UC3["(Sélectionner une tâche)"]
    UC4["(Déplacer la tâche\nvers la colonne suivante)"]
    UC5["(Mettre à jour le statut\nde la tâche)"]

    DEV --> UC1
    SM --> UC1
    PO --> UC1
    UC1 --> UC2
    DEV --> UC3
    UC3 --> UC4
    UC4 --> UC5
```

> **Post-conditions** : Le statut de la tâche est mis à jour en temps réel dans le Sprint Backlog.

---

## 🗺️ Diagramme de synthèse — Vue globale du Core Domain

```mermaid
---
title: Project & Sprint Planning — Vue Globale des Cas d'Utilisation
---
flowchart TB
    subgraph Acteurs
        PO(["👤 Product Owner"])
        SM(["👤 Scrum Master"])
        DEV(["👤 Développeur"])
        ADM(["👤 Administrateur"])
        SYS(["⚙️ Système"])
    end

    subgraph UC_PROJECT["📁 Gestion de Projet"]
        UC41["UC-4.1\nCréer un Projet"]
        UC42["UC-4.2\nCréer un Epic"]
        UC43["UC-4.3\nCréer une User Story"]
        UC44["UC-4.4\nEstimer une User Story"]
    end

    subgraph UC_SPRINT["🏃 Gestion de Sprint"]
        UC45["UC-4.5\nPlanifier un Sprint"]
        UC46["UC-4.6\nDéfinir le Sprint Goal"]
        UC47["UC-4.7\nClôturer un Sprint"]
        UC48["UC-4.8\nModifier le périmètre du Sprint"]
    end

    subgraph UC_KANBAN["📋 Suivi Kanban"]
        UC49["UC-4.9\nConsulter le tableau Kanban"]
    end

    PO --> UC41
    ADM --> UC41
    PO --> UC42
    PO --> UC43
    DEV --> UC44
    SM --> UC44

    SM --> UC45
    PO --> UC45
    PO --> UC46
    SM --> UC47
    PO --> UC48

    DEV --> UC49
    SM --> UC49
    PO --> UC49

    SYS --> UC44
    SYS --> UC45
    SYS --> UC47

    UC41 --> UC42
    UC42 --> UC43
    UC43 --> UC44
    UC44 --> UC45
    UC46 -.->|"«include»"| UC45
    UC45 --> UC47
    UC47 --> UC48
```

---

## 🔗 Interactions avec les autres Bounded Contexts

```mermaid
---
title: Événements de domaine émis par Project & Sprint Planning
---
flowchart LR
    PROJ["📅 Project & Sprint Planning\n(Core Domain)"]

    MON["📊 Monitoring & Reporting"]
    ART["🧱 Scrum Artifacts"]

    PROJ -- "SprintClos 🚀" --> MON
    PROJ -- "SprintClos 🚀" --> ART

    style PROJ fill:#2563eb,color:#fff,stroke:#1d4ed8
    style MON fill:#0891b2,color:#fff,stroke:#0e7490
    style ART fill:#7c3aed,color:#fff,stroke:#6d28d9
```

---

## 📚 Légende des relations UML

| Notation | Signification |
|---|---|
| `-->` | Association (acteur déclenche le cas d'utilisation) |
| `-.->` avec `«include»` | Le UC inclut obligatoirement l'autre UC |
| `-.->` avec `«extend»` | Le UC étend un autre UC dans un cas particulier |
| Sous-graphe | Regroupement thématique (package UML) |

---

> *Document généré pour le projet **ScrumFlow** — Matière : Développement Avancé (Microservices Java / Spring Boot — DDD)*
