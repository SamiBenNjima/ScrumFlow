# ScrumFlow — Cas d'Utilisation (Use Cases)

> Ce document détaille l'ensemble des cas d'utilisation de l'application **ScrumFlow**, organisés par Bounded Context (domaine métier), conformément à l'architecture définie dans le [README.md](README.md).

---

## 📋 Légende

- **Acteur principal** : rôle déclenchant le cas d'utilisation.
- **Acteurs secondaires** : rôles ou systèmes impliqués.
- **Préconditions** : état requis avant l'exécution.
- **Scénario nominal** : déroulement standard.
- **Extensions / Exceptions** : cas alternatifs ou d'erreur.
- **Post-conditions** : état du système après exécution.

### 🎭 Rôles principaux

| Rôle | Description |
|---|---|
| **Administrateur** | Gère les comptes, rôles et la configuration globale |
| **Product Owner (PO)** | Représente les besoins métier, priorise le backlog |
| **Scrum Master (SM)** | Facilite le processus Scrum, lève les obstacles |
| **Développeur** | Membre de l'équipe technique réalisant les user stories |
| **Partie Prenante** | Client, sponsor ou toute personne intéressée par le projet |

---

## 1. 🔐 Identity & Access Management

### UC-1.1 — Créer un compte utilisateur
- **Acteur principal** : Administrateur
- **Préconditions** : L'administrateur est authentifié.
- **Scénario nominal** :
  1. L'administrateur accède au module de gestion des utilisateurs.
  2. Il saisit les informations du nouvel utilisateur (nom, email, rôle).
  3. Le système valide l'unicité de l'email.
  4. Le système crée le compte et envoie un email d'activation.
- **Extensions** : Email déjà utilisé → message d'erreur, saisie refusée.
- **Post-conditions** : Un nouveau compte `Utilisateur` est créé à l'état "inactif".

### UC-1.2 — S'authentifier (connexion)
- **Acteur principal** : Tout utilisateur enregistré
- **Scénario nominal** :
  1. L'utilisateur saisit son identifiant et mot de passe.
  2. Le système vérifie les identifiants.
  3. Le système génère un jeton de session (token).
  4. L'utilisateur accède à son tableau de bord selon son rôle.
- **Extensions** : Identifiants invalides → message d'erreur ; compte verrouillé après N tentatives.

### UC-1.3 — Se déconnecter
- **Acteur principal** : Utilisateur connecté
- **Scénario nominal** : L'utilisateur demande la déconnexion ; le système invalide le token de session.

### UC-1.4 — Gérer les rôles et permissions
- **Acteur principal** : Administrateur
- **Scénario nominal** :
  1. L'administrateur sélectionne un utilisateur.
  2. Il modifie son rôle (Admin, Scrum Master, Product Owner, Développeur).
  3. Le système met à jour les permissions associées.
- **Post-conditions** : Les droits d'accès de l'utilisateur sont recalculés immédiatement.

### UC-1.5 — Modifier son profil
- **Acteur principal** : Utilisateur connecté
- **Scénario nominal** : L'utilisateur modifie ses informations personnelles (nom, avatar, préférences) et enregistre.

### UC-1.6 — Réinitialiser son mot de passe
- **Acteur principal** : Utilisateur
- **Scénario nominal** :
  1. L'utilisateur demande une réinitialisation via son email.
  2. Le système envoie un lien sécurisé à durée limitée.
  3. L'utilisateur définit un nouveau mot de passe.

---

## 2. 🤝 Stakeholder Management

### UC-2.1 — Enregistrer une partie prenante
- **Acteur principal** : Product Owner
- **Scénario nominal** :
  1. Le PO crée une fiche `PartiePrenante` (nom, type : client/sponsor, coordonnées).
  2. Le PO définit son niveau d'intérêt et d'influence.
  3. Le système enregistre la fiche et l'associe au projet.

### UC-2.2 — Classifier les parties prenantes
- **Acteur principal** : Product Owner
- **Scénario nominal** : Le PO positionne chaque partie prenante sur une matrice Intérêt/Influence afin d'adapter la stratégie de communication.

### UC-2.3 — Suivre les attentes d'une partie prenante
- **Acteur principal** : Product Owner
- **Scénario nominal** : Le PO consigne les attentes exprimées et les relie aux epics/user stories concernées.

### UC-2.4 — Modifier / archiver une partie prenante
- **Acteur principal** : Product Owner
- **Scénario nominal** : Le PO met à jour les informations ou archive une partie prenante devenue inactive.

---

## 3. 👥 Team Management

### UC-3.1 — Créer une équipe Scrum
- **Acteur principal** : Administrateur / Scrum Master
- **Scénario nominal** :
  1. Création d'une `Equipe` avec un nom et un projet associé.
  2. Ajout des membres disponibles.
  3. Attribution des rôles Scrum (Scrum Master, Product Owner, Développeurs).

### UC-3.2 — Affecter un membre à une équipe
- **Acteur principal** : Scrum Master
- **Scénario nominal** : Le SM sélectionne un utilisateur et l'affecte à l'équipe avec un rôle Scrum donné.
- **Extensions** : Le membre est déjà alloué à 100% sur un autre projet → alerte de surallocation.

### UC-3.3 — Gérer la disponibilité / capacité d'un membre
- **Acteur principal** : Membre de l'équipe / Scrum Master
- **Scénario nominal** : Déclaration des congés, absences ou capacité de travail (%) pour le sprint à venir, utilisée dans le calcul de capacité d'équipe.

### UC-3.4 — Retirer un membre d'une équipe
- **Acteur principal** : Scrum Master
- **Scénario nominal** : Le SM retire un membre ; ses tâches en cours doivent être réassignées.

---

## 4. 📅 Project & Sprint Planning *(Core Domain)*

### UC-4.1 — Créer un projet
- **Acteur principal** : Product Owner / Administrateur
- **Scénario nominal** :
  1. Saisie des informations du projet (nom, description, dates, équipe).
  2. Association d'un budget initial et de parties prenantes.
  3. Le système crée l'agrégat `Projet`.

### UC-4.2 — Créer un Epic
- **Acteur principal** : Product Owner
- **Scénario nominal** : Le PO définit un Epic (grande fonctionnalité) rattaché au projet, avec une description et une valeur métier.

### UC-4.3 — Créer une User Story
- **Acteur principal** : Product Owner
- **Scénario nominal** :
  1. Le PO rédige la user story (« En tant que... je veux... afin de... »).
  2. Il définit les critères d'acceptation.
  3. Il la rattache éventuellement à un Epic.

### UC-4.4 — Estimer une User Story (Story Points)
- **Acteur principal** : Équipe de développement
- **Acteurs secondaires** : Scrum Master (facilitateur)
- **Scénario nominal** :
  1. L'équipe discute la story lors du Refinement/Planning.
  2. Chaque développeur propose une estimation (ex. Planning Poker).
  3. Le système enregistre le consensus en Story Points.

### UC-4.5 — Planifier un Sprint (Sprint Planning)
- **Acteur principal** : Scrum Master / Product Owner
- **Acteurs secondaires** : Équipe de développement
- **Préconditions** : Le backlog produit est priorisé et estimé.
- **Scénario nominal** :
  1. Création d'un `Sprint` avec dates de début/fin et objectif (Sprint Goal).
  2. Sélection des user stories du backlog en fonction de la capacité de l'équipe.
  3. Constitution du Sprint Backlog.
- **Post-conditions** : Le sprint passe à l'état "Actif".

### UC-4.6 — Définir le Sprint Goal
- **Acteur principal** : Product Owner
- **Scénario nominal** : Le PO formule l'objectif du sprint, validé collectivement avec l'équipe.

### UC-4.7 — Clôturer un Sprint (Sprint Review + Retrospective)
- **Acteur principal** : Scrum Master
- **Scénario nominal** :
  1. Le SM déclenche la clôture du sprint.
  2. Le système calcule les stories terminées / non terminées.
  3. Les stories non terminées sont proposées pour réintégration au backlog produit.
  4. Un événement de domaine `SprintClos` est émis (consommé par Monitoring & Reporting).
- **Post-conditions** : Le sprint passe à l'état "Terminé" ; un rapport de sprint est généré.

### UC-4.8 — Modifier le périmètre d'un sprint en cours
- **Acteur principal** : Product Owner
- **Scénario nominal** : Ajout/retrait exceptionnel d'une story en cours de sprint, avec justification (ex. urgence).
- **Extensions** : Alerte si cela dépasse la capacité de l'équipe.

### UC-4.9 — Consulter le tableau Kanban du sprint
- **Acteur principal** : Développeur
- **Scénario nominal** : Le développeur consulte les colonnes (À faire / En cours / En revue / Terminé) et déplace les tâches selon leur avancement.

---

## 5. 🧱 Scrum Artifacts

### UC-5.1 — Gérer le Product Backlog
- **Acteur principal** : Product Owner
- **Scénario nominal** :
  1. Le PO ajoute, modifie ou supprime des éléments du backlog produit.
  2. Le PO priorise les items (ordre de valeur métier).

### UC-5.2 — Affiner le backlog (Backlog Refinement)
- **Acteur principal** : Product Owner
- **Acteurs secondaires** : Équipe de développement
- **Scénario nominal** : Session collaborative de clarification, découpage et estimation des items à venir.

### UC-5.3 — Générer le Sprint Backlog
- **Acteur principal** : Système (déclenché lors du Sprint Planning)
- **Scénario nominal** : À partir des stories sélectionnées (UC-4.5), le système constitue le Sprint Backlog avec les tâches associées.

### UC-5.4 — Décomposer une User Story en tâches techniques
- **Acteur principal** : Développeur
- **Scénario nominal** : Le développeur découpe une story en tâches techniques (ex. « créer l'API », « écrire les tests »).

### UC-5.5 — Suivre l'Increment livré
- **Acteur principal** : Scrum Master / Product Owner
- **Scénario nominal** : À la fin du sprint, le système consolide les stories "Terminées" en un Increment, potentiellement livrable.

---

## 6. 📦 Deliverable Management

### UC-6.1 — Créer un livrable
- **Acteur principal** : Product Owner / Développeur
- **Scénario nominal** : Création d'un `Livrable` rattaché à un jalon ou une release, avec version et description.

### UC-6.2 — Soumettre un livrable pour validation
- **Acteur principal** : Développeur
- **Scénario nominal** :
  1. Le développeur soumet le livrable (état "En attente de validation").
  2. Notification envoyée au Product Owner / partie prenante concernée.

### UC-6.3 — Valider ou rejeter un livrable
- **Acteur principal** : Product Owner
- **Acteurs secondaires** : Partie Prenante
- **Scénario nominal** :
  1. Le PO examine le livrable soumis.
  2. Il approuve (→ état "Validé") ou rejette avec commentaires (→ état "Rejeté").
- **Post-conditions (si validé)** : Un événement `LivrableValidé` est émis (consommé par Financial Management / Monitoring).

### UC-6.4 — Publier une Release
- **Acteur principal** : Product Owner
- **Préconditions** : Tous les livrables associés sont validés.
- **Scénario nominal** : Le PO regroupe les livrables validés en une release et la publie officiellement.

### UC-6.5 — Suivre les jalons (Milestones)
- **Acteur principal** : Scrum Master
- **Scénario nominal** : Définition de jalons clés du projet et suivi de leur atteinte par rapport au planning.

---

## 7. 🛠️ Resource & Material Management

### UC-7.1 — Gérer l'inventaire du matériel
- **Acteur principal** : Administrateur / Gestionnaire de ressources
- **Scénario nominal** : Ajout, modification, suppression d'un `Ressource` (matériel : licences, machines, salles).

### UC-7.2 — Allouer une ressource à un projet
- **Acteur principal** : Gestionnaire de ressources
- **Scénario nominal** :
  1. Sélection d'une ressource disponible.
  2. Allocation à un projet pour une période donnée.
- **Extensions** : Ressource déjà réservée sur la période → conflit signalé.

### UC-7.3 — Réserver une ressource matérielle
- **Acteur principal** : Membre d'équipe
- **Scénario nominal** : Réservation ponctuelle d'une ressource (ex. salle de réunion, matériel de test).

### UC-7.4 — Suivre la disponibilité des ressources humaines et matérielles
- **Acteur principal** : Scrum Master
- **Scénario nominal** : Consultation d'un calendrier consolidé des disponibilités (personnes + matériel).

### UC-7.5 — Libérer une ressource
- **Acteur principal** : Gestionnaire de ressources
- **Scénario nominal** : Fin d'allocation d'une ressource en fin de projet ou de sprint, la rendant disponible pour d'autres projets.

---

## 8. 💰 Financial Management

### UC-8.1 — Définir le budget d'un projet
- **Acteur principal** : Product Owner / Administrateur
- **Scénario nominal** : Définition du `Budget` initial alloué au projet (montant, devise, répartition par poste).

### UC-8.2 — Enregistrer une dépense
- **Acteur principal** : Product Owner / Gestionnaire financier
- **Scénario nominal** :
  1. Saisie d'une dépense (montant, catégorie, justificatif).
  2. Le système impute la dépense au budget du projet.
- **Extensions** : Dépense dépassant le budget restant → alerte d'écart budgétaire.

### UC-8.3 — Suivre l'écart budgétaire
- **Acteur principal** : Product Owner
- **Scénario nominal** : Consultation d'un tableau de bord comparant budget prévu vs dépenses réelles, avec calcul de l'écart.

### UC-8.4 — Générer une facture
- **Acteur principal** : Gestionnaire financier
- **Acteurs secondaires** : Partie Prenante (client)
- **Scénario nominal** : Génération d'une facture liée à une release ou un jalon livré, envoyée à la partie prenante concernée.

### UC-8.5 — Clôturer le budget en fin de projet
- **Acteur principal** : Product Owner
- **Scénario nominal** : Bilan financier final du projet (budget consommé, écart final, archivage).

---

## 9. ⚠️ Risk Management

### UC-9.1 — Identifier un risque
- **Acteur principal** : Scrum Master / tout membre de l'équipe
- **Scénario nominal** : Création d'un `Risque` avec description, catégorie, probabilité et impact estimés.

### UC-9.2 — Évaluer un risque
- **Acteur principal** : Scrum Master
- **Scénario nominal** : Calcul de la criticité du risque (probabilité × impact) et classement (faible/moyen/élevé).

### UC-9.3 — Définir un plan de mitigation
- **Acteur principal** : Scrum Master
- **Scénario nominal** : Association d'actions préventives/correctives à un risque identifié, avec responsable et échéance.

### UC-9.4 — Suivre l'évolution des risques
- **Acteur principal** : Scrum Master / Product Owner
- **Scénario nominal** : Revue périodique des risques ouverts (mise à jour de probabilité/impact, clôture si résolu).

### UC-9.5 — Clôturer un risque
- **Acteur principal** : Scrum Master
- **Scénario nominal** : Passage du risque à l'état "Clôturé" une fois le plan de mitigation exécuté ou le risque devenu obsolète.

---

## 10. 📊 Monitoring & Reporting

### UC-10.1 — Calculer la vélocité de l'équipe
- **Acteur principal** : Système (déclenché par `SprintClos`)
- **Acteurs secondaires** : Scrum Master (consultation)
- **Scénario nominal** : À la clôture d'un sprint, le système calcule la vélocité (somme des story points terminés) et l'historise.

### UC-10.2 — Générer un Burndown Chart
- **Acteur principal** : Scrum Master / Équipe
- **Scénario nominal** : Le système affiche l'avancement quotidien du travail restant sur le sprint courant comparé à la trajectoire idéale.

### UC-10.3 — Générer un Burnup Chart
- **Acteur principal** : Product Owner
- **Scénario nominal** : Le système affiche l'avancement cumulatif du travail terminé par rapport au périmètre total du projet.

### UC-10.4 — Consulter le tableau de bord projet (KPI)
- **Acteur principal** : Product Owner / Partie Prenante
- **Scénario nominal** : Consultation d'indicateurs consolidés : vélocité moyenne, budget consommé, risques actifs, livrables validés.

### UC-10.5 — Générer un rapport de sprint
- **Acteur principal** : Scrum Master
- **Scénario nominal** : Génération automatique d'un `RapportSprint` à la clôture du sprint (stories terminées, vélocité, obstacles rencontrés).

### UC-10.6 — Exporter un rapport
- **Acteur principal** : Product Owner
- **Scénario nominal** : Export d'un rapport (PDF/Excel) pour diffusion aux parties prenantes externes.

---

## 🔗 Synthèse des interactions inter-domaines (événements)

| Événement de domaine | Émis par | Consommé par |
|---|---|---|
| `SprintClos` | Project & Sprint Planning | Monitoring & Reporting, Scrum Artifacts |
| `LivrableValidé` | Deliverable Management | Financial Management, Monitoring & Reporting |
| `UtilisateurCréé` | Identity & Access Management | Team Management |
| `RessourceAllouée` | Resource & Material Management | Financial Management (imputation coût) |
| `RisqueCritiqueDétecté` | Risk Management | Monitoring & Reporting |

---

## 📚 Contexte académique

Ce document complète le [README.md](README.md) dans le cadre de la matière **Développement Avancé** (Microservices — Java / Spring Boot), en détaillant les cas d'utilisation issus de l'analyse **Domain-Driven Design** de chaque Bounded Context.
