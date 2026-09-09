# OPAL — Guide Utilisateur

Bienvenue dans OPAL (OMOP Platform for Analytics & Lineage). Ce guide vous accompagne dans l'utilisation de chaque fonctionnalite de la plateforme.

---

## Table des matieres

1. [Premiers pas](#1-premiers-pas)
2. [Navigation et interface](#2-navigation-et-interface) — notifications, recherche globale, favoris, requetes sauvegardees, modeles
3. [Gestion des CDM](#3-gestion-des-cdm) — enregistrement, schemas par categorie
4. [Analyse Qualite](#4-analyse-qualite) — analyse par domaine, conformite, comparaison, rapports
5. [Constructeur de Cohortes](#5-constructeur-de-cohortes) — Query Builder, **Assistant IA**, concept sets, Table 1, parcours patient, pathways, incidence, estimation
6. [Workflow de Mapping](#6-workflow-de-mapping) — 6 onglets, consensus, application, rollback
7. [Charger un referentiel (CCAM, CIM-10...)](#7-charger-un-referentiel-ccam-cim-10)
8. [Data Management — extraire un jeu de donnees](#8-data-management--extraire-un-jeu-de-donnees)
9. [Lineage — lignage ETL](#9-lineage--lignage-etl)
10. [Explorateur de Concepts](#10-explorateur-de-concepts)
11. [Outils OHDSI](#11-outils-ohdsi)
12. [Parametres](#12-parametres) — Source Value Cache, SapBERT, LLM on-premise
13. [Audit et Administration](#13-audit-et-administration) — utilisateurs, groupes, acces CDM, partage
14. [FAQ et Depannage](#14-faq-et-depannage)

---

## 1. Premiers pas

### Connexion

#### Avec authentification (Keycloak active)

1. Ouvrir OPAL dans votre navigateur : `http://<adresse>:3000`
2. Cliquer sur **Se connecter via Keycloak**
3. Saisir vos identifiants sur la page Keycloak
4. Vous etes redirige vers OPAL avec votre session active

Si vous n'avez pas de compte :

1. Cliquer sur l'onglet **Inscription**
2. Remplir le formulaire : **identifiant professionnel** et **role souhaite**
3. Soumettre la demande
4. Attendre la validation par un administrateur
5. Vous vous connecterez ensuite avec vos identifiants une fois approuve

#### Sans authentification

Si Keycloak n'est pas active (`AUTH_ENABLED=true` par defaut, a mettre a `false` pour desactiver), vous accedez directement a toutes les fonctionnalites en tant qu'administrateur.

### Roles et permissions

Votre role determine les pages et fonctionnalites auxquelles vous avez acces :

| Role | Description | Pages accessibles |
|------|-------------|-------------------|
| **Admin** | Administrateur systeme | Toutes les pages + Audit + Gestion utilisateurs |
| **OMOP DIM** | Data steward | Toutes les pages fonctionnelles |
| **Chercheur** | Recherche clinique | Qualite, Cohortes, Concepts |
| **Medecin** | Medecin / terminologue | Mapping, Cohortes, Concepts |

Votre role est affiche sous votre nom d'utilisateur dans la barre de navigation supérieure (TopNav).

### Selectionner un CDM

Avant d'utiliser la plupart des fonctionnalites, vous devez selectionner un CDM :

1. Dans la barre de navigation supérieure (TopNav), deployer le selecteur **CDM**
2. Choisir le CDM sur lequel vous souhaitez travailler
3. Le CDM selectionne est retenu entre les sessions

> **Note** : Si aucun CDM n'est enregistre, demandez a un administrateur d'en configurer un (page Gestion des CDM).

---

## 2. Navigation et interface

### Barre de navigation supérieure (TopNav)

La barre de navigation supérieure est votre point d'acces a toutes les fonctionnalites :

- **Logo OPAL** : retour a la page d'accueil
- **Selecteur CDM** : choisir la base OMOP active
- **Qualite** : analyse qualite des donnees
- **Cohortes** : constructeur de cohortes
- **Mapping** : workflow de mapping des vocabulaires
- **Concepts** : explorateur de concepts OMOP
- **Gestion CDM** : enregistrement des connexions (admin)
- **OHDSI** : outils OHDSI (admin)
- **Parametres** : configuration (admin)
- **Audit** : journal d'activite (admin)
- **Utilisateurs** : gestion des comptes (admin)

A droite de la TopNav :
- **Bouton langue** (FR/EN) : basculer entre francais et anglais
- **Mode sombre** : activer/desactiver le theme sombre
- **Cloche notifications** : voir les notifications non lues
- **Deconnexion** : fermer votre session

### Page d'accueil (Dashboard)

La page d'accueil s'affiche apres connexion et offre une vue d'ensemble de votre environnement OPAL :

- **CDM enregistres** : liste des bases OMOP disponibles avec leurs informations principales
- **Activites recentes** : dernieres actions effectuees (analyses, cohortes, mappings)
- **Favoris** : acces rapide aux elements marques comme favoris
- **Acces rapide** : raccourcis vers les fonctionnalites principales (Qualite, Cohortes, Mapping, Concepts)

### Notifications

L'icone cloche dans la TopNav affiche le nombre de notifications non lues :

- Cliquer sur la cloche pour ouvrir le **tiroir de notifications**
- Les notifications sont classees par categorie :
  - **Analyses** : fin d'analyse qualite, nouveaux snapshots
  - **Cohortes** : partage de cohortes, mises a jour
  - **Mapping** : suggestions generees, decisions appliquees
  - **Administration** : demandes d'acces, changements de role
- Actions disponibles : **marquer comme lu**, **supprimer** une notification
- Les notifications arrivent en **temps reel via WebSocket** (pas besoin de rafraichir la page)

### Preferences de notification

Chaque utilisateur choisit les **types** de notification qu'il souhaite recevoir.
Depuis le tiroir de notifications, ouvrir les preferences et activer/desactiver
chaque type independamment. Un type desactive n'est plus notifie ; les
notifications deja recues restent consultables.

### Recherche globale

La barre de recherche de la TopNav interroge **toutes les entites a la fois** :
cohortes, concept sets, requetes sauvegardees, concepts OMOP du CDM courant.

- Les resultats sont groupes par type
- Cliquer sur un resultat ouvre directement l'element
- La recherche peut etre restreinte au CDM selectionne

### Favoris

N'importe quelle cohorte, requete, concept ou CDM peut etre mis en **favori**
(icone etoile). Les favoris sont **personnels** et se retrouvent depuis la page
d'accueil, pour revenir en un clic aux elements utilises souvent.

### Requetes SQL sauvegardees

Dans l'editeur SQL du constructeur de cohortes, une requete peut etre
**sauvegardee** avec un nom et une description, puis rechargee, modifiee ou
supprimee. Les requetes sont **personnelles** et rattachees a un CDM.

### Modeles de cohortes

Les **modeles** (templates) sont des definitions de criteres reutilisables,
classees par categorie. OPAL en fournit un jeu integre ; les administrateurs et
OMOP DIM peuvent en creer d'autres.

1. Depuis le constructeur, choisir un modele comme point de depart
2. Les criteres du modele sont charges dans le Query Builder
3. Les adapter, puis enregistrer comme une cohorte normale

### Conventions d'interface

- Les **boutons bleus** declenchent des actions principales
- Les **tags colores** indiquent des statuts (vert = succes, orange = avertissement, rouge = erreur)
- Les **icones d'export** (telechargement) permettent d'exporter en CSV
- Les **indicateurs de chargement** (spinners) indiquent un traitement en cours

---

## 3. Gestion des CDM

**Acces** : Admin, OMOP DIM

Cette page permet d'enregistrer et gerer les connexions aux bases OMOP CDM externes.

### Enregistrer un nouveau CDM

1. Acceder a la page **Gestion des CDM**
2. Remplir le formulaire :
   - **Nom** : identifiant unique (ex: `CHU_OMOP_PROD`)
   - **Hote** : adresse du serveur PostgreSQL
   - **Port** : port PostgreSQL (defaut : 5432)
   - **Base de donnees** : nom de la base
   - **Utilisateur** : compte PostgreSQL
   - **Mot de passe** : sera chiffre avant stockage
   - **Schema OMOP** : schema contenant les tables OMOP (defaut : `omop_cdm`)
3. Cliquer sur **Tester la connexion** pour verifier les parametres
4. Si le test est reussi (nombre de patients affiche), cliquer sur **Enregistrer**

### Gerer les CDM existants

La liste des CDM enregistres affiche :
- Nom, hote, port, base, utilisateur, schema
- Bouton **Tester** : verifier que la connexion fonctionne toujours
- Bouton **Supprimer** : retirer le CDM (avec confirmation)

> **Important** : Le mot de passe n'est jamais affiche. Il est chiffre avec Fernet (AES-128) dans la base.

---

### Schemas par categorie (avance)

Par defaut, toutes les tables OMOP sont lues dans le **schema OMOP** unique
renseigne a l'enregistrement du CDM. Certaines installations rangent les tables
dans **plusieurs schemas** — typiquement un **vocabulaire partage** entre
plusieurs CDM.

La section repliable **Per-category schemas (advanced)** permet de surcharger le
schema **par categorie de tables** de la norme OMOP CDM v5.4 :

| Categorie | Exemples de tables |
|---|---|
| `clinical` | `person`, `condition_occurrence`, `drug_exposure` |
| `health_system` | `care_site`, `provider`, `location` |
| `health_economics` | `cost`, `payer_plan_period` |
| `derived` | `condition_era`, `drug_era` |
| `metadata` | `cdm_source`, `metadata` |
| `vocabulary` | `concept`, `concept_relationship`, `concept_ancestor` |

- Laisser un champ **vide** = la categorie utilise le schema par defaut
- Le nombre de tables concernees est indique en face de chaque categorie
- Les reglages du CDM priment sur ceux de la configuration initiale

> Utile par exemple pour pointer `vocabulary` vers un schema `omop_vocab`
> commun, tout en gardant les donnees cliniques dans le schema de l'etablissement.

---

## 4. Analyse Qualite

**Acces** : Admin, OMOP DIM, Chercheur

L'analyse qualite evalue vos donnees OMOP a travers 14 domaines (3 speciaux + 11 cliniques) et stocke les resultats sous forme de snapshots versiones.

### Lancer une analyse

#### Analyse d'un seul domaine

1. Selectionner un **domaine** dans la liste a gauche :
   - **Dashboard** : vue d'ensemble globale
   - **Person** : demographie
   - **ObservationPeriod** : periodes d'observation
   - **Condition, Drug, Measurement, Observation, Procedure, Visit, Device, Death, Specimen, Note, Payer_Plan_Period** : domaines cliniques
2. Cliquer sur **Analyser**
3. Les resultats s'affichent avec graphiques et tableaux

#### Analyse par lot (tous les domaines)

1. Cliquer sur **Analyse complete**
2. Une barre de progression montre l'avancement domaine par domaine
3. Chaque domaine est coche une fois termine
4. Vous pouvez annuler en cours de route

### Comprendre les resultats

#### Dashboard

- **Total personnes** : nombre de patients dans le CDM
- **Tableau par domaine** : pour chaque domaine, le nombre de records, personnes, et taux de mapping
- **Vue globale** : couverture des donnees a travers les domaines

#### Person (demographie)

- **Distribution par genre** : camembert montrant la repartition H/F/Autre
- **Annees de naissance** : histogramme des annees de naissance
- **Distribution par race** : camembert (si disponible)
- **Distribution par ethnicite** : camembert (si disponible)

#### Observation Period

- **Age a la premiere observation** : histogramme de l'age au debut du suivi
- **Age par genre** : quantiles (p10, p25, mediane, p75, p90) par genre
- **Duree d'observation** : histogramme de la duree en mois
- **Observation cumulative** : courbe montrant le % de patients avec au moins X mois de suivi
- **Observation continue par annee** : nombre de patients observes chaque annee

#### Domaines cliniques (Condition, Drug, etc.)

- **Statistiques globales** : total lignes, personnes, moyenne par personne
- **Evolution mensuelle** : courbe du nombre de records par mois
- **Distribution records/personne** : histogramme
- **Top concepts** : tableau des concepts les plus frequents
- **Qualite du mapping** :
  - Taux de mapping au niveau terme (combien de codes source distincts sont mappes)
  - Taux de mapping au niveau ligne (combien de lignes sont mappees)
  - Top termes non mappes avec leur frequence

### Conformite du CDM

En complement de l'analyse par domaine, l'onglet **Conformite** lance une
**validation structurelle** du CDM : les tables et colonnes attendues sont-elles
presentes, les valeurs sont-elles coherentes ?

Les controles sont regroupes en quatre categories :

| Categorie | Ce qui est verifie |
|---|---|
| **Structure** | Presence des tables et colonnes attendues de la norme OMOP |
| **Conformance** | Respect des conventions OMOP (types, references de concepts) |
| **Completeness** | Champs obligatoires renseignes, taux de valeurs manquantes |
| **Plausibility** | Valeurs vraisemblables (dates coherentes, ages, intervalles) |

1. Lancer la validation (elle tourne en tache de fond et peut etre **annulee**)
2. Chaque controle ressort avec un **statut** (succes / avertissement / echec)
   et son detail
3. Un **score global** resume la conformite
4. Le dernier resultat est conserve : il est recharge automatiquement a la
   reouverture de la page, sans relancer le calcul

> La validation est limitee a 3 lancements par minute.

### Exporter les resultats

Chaque tableau peut etre exporte en CSV :
- Cliquer sur l'icone de telechargement a cote du tableau
- Le fichier CSV est telecharge automatiquement

### Historique des snapshots

Chaque analyse cree une nouvelle version :
- Le selecteur de version (v1, v2, v3...) permet de charger un snapshot anterieur
- La date de creation est affichee

### Comparer deux CDM

1. Cliquer sur le bouton **Comparer** ou activer le mode comparaison
2. Selectionner les deux CDM a comparer
3. Choisir un domaine
4. Les resultats s'affichent cote a cote avec les ecarts en pourcentage
5. Les alertes sont colorees :
   - **Jaune** : ecart > seuil (defaut 5%)
   - **Rouge** : ecart > 2x le seuil (defaut 10%)

### Generer un rapport

1. Cliquer sur **Rapport HTML** ou **Rapport PDF**
2. Choisir la langue (Francais ou Anglais)
3. Le rapport compile tous les domaines analyses
4. Pour un rapport de comparaison, cliquer sur **Rapport comparaison**

---

## 5. Constructeur de Cohortes

**Acces** : Admin, OMOP DIM, Chercheur, Medecin

Le constructeur de cohortes permet de definir visuellement des populations de patients a partir de criteres cliniques.

### Interface

L'ecran est divise en 3 zones :

| Zone | Position | Role |
|------|----------|------|
| **Criteres** | Gauche | Recherche et selection de concepts OMOP |
| **Canvas** | Centre | Construction de la requete (4 onglets) |
| **Resultats** | Droite | Comptage, attrition, echantillon, SQL |

### Creer une cohorte

#### Etape 1 : Rechercher des concepts

1. Dans le panneau de gauche, selectionner un **domaine** (Condition, Drug, etc.)
2. Taper un terme de recherche (ex: "diabete", "metformine")
3. Optionnellement filtrer par **vocabulaire** (SNOMED, ICD10, ATC...)
4. Les concepts correspondants s'affichent dans une liste
5. Cliquer sur un concept pour l'ajouter comme critere d'inclusion

#### Etape 2 : Configurer les criteres

Dans le **Query Builder** (onglet central) :

**Criteres d'inclusion** :
- Chaque critere est affiche comme un bloc avec le nom du concept
- **Inclure descendants** (coche) : inclut automatiquement tous les concepts enfants dans la hierarchie OMOP
- **Operateur** entre criteres : AND (les deux doivent etre vrais) ou OR (l'un ou l'autre)

**Contraintes temporelles** (par critere) :
- **Tout moment** : pas de restriction de date
- **Fenetre absolue** : entre une date de debut et une date de fin
- **Jours relatifs** : dans les N derniers jours

**Contraintes de frequence** (par critere) :
- **Au moins N fois** : le patient doit avoir au moins N occurrences
- **Exactement N fois** : exactement N occurrences
- **Au plus N fois** : pas plus de N occurrences
- **Fenetre glissante** : N occurrences dans les X jours

**Contraintes de valeur** (Measurement uniquement) :
- Operateur : `>`, `<`, `>=`, `<=`, `=`, `entre`
- Valeur numerique et unite

**Criteres d'exclusion** :
- Ajoutez des criteres dans la section exclusion
- Les patients correspondant a ces criteres seront retires du resultat

**Criteres demographiques** :
- **Age** : min et max
- **Genre** : selectionner un ou plusieurs genres
- **Race** et **Ethnicite** : filtres optionnels

#### Etape 3 : Executer

Dans le panneau de droite :

- **Compter** : affiche le nombre de patients correspondants
- **Comptage rapide** : estimation rapide (moins precis mais plus rapide)
- **Attrition** : montre le nombre de patients a chaque etape (critere par critere), pour comprendre l'impact de chaque critere
- **Echantillon** : affiche 10 patients aleatoires avec leur demographie
  - Cliquer sur un `person_id` pour voir le **parcours patient** (timeline de tous les evenements cliniques)

#### Etape 4 : Sauvegarder

1. Donner un **nom** et une **description** a la cohorte
2. Cliquer sur **Sauvegarder**
3. La cohorte apparait dans la liste des cohortes sauvegardees
4. Chaque modification des criteres cree une **nouvelle version**

### Caracterisation (Table 1)

L'onglet **Table 1** genere une caracterisation complete de la cohorte :

1. Definir vos criteres dans le Query Builder
2. Cliquer sur l'onglet **Table 1**
3. Cliquer sur **Generer la caracterisation**
4. Les resultats incluent :
   - **Demographie** : statistiques d'age (moyenne, mediane, ecart-type), repartition par genre, race, ethnicite
   - **Prevalence par domaine** : pourcentage de patients avec des donnees dans chaque domaine + top concepts
   - **Mesures** : statistiques des mesures biologiques (moyenne, mediane, min, max)
   - **Types de visite** : repartition des types de visite (ambulatoire, hospitalisation, urgences...)
   - **Periodes d'observation** : statistiques de duree de suivi

### Organisation des onglets

La page Cohortes est organisee en **deux groupes d'onglets** :

| Groupe | Onglets |
|---|---|
| **Cohort Builder** | Assistant IA*, Query Builder, Table 1, SQL, Concept Sets |
| **Analyse** | Compare, Pathways, Incidence, Estimation |

\* L'onglet **Assistant IA** n'apparait que si la fonctionnalite est activee
cote serveur (`COHORT_LLM_MODE`).

### Assistant IA — construire une cohorte en langage naturel

**Acces** : onglet *Cohort Builder → Assistant IA* (visible seulement si active)

Decrivez la cohorte en francais ; l'assistant produit un **brouillon** dont
chaque terme est resolu en **codes reels de votre CDM**.

1. Saisir la description, par exemple :
   > « Femmes de 50 à 70 ans diabétiques de type 2 sous metformine, hospitalisées depuis 2022 »
2. Cliquer sur **Generer**
3. Le brouillon s'affiche : **demographie** (age, sexe, periode) + **criteres**,
   chaque critere accompagne des **concept-sets** proposes (codes du CDM)
4. **Revoir chaque critere** : cocher les codes pertinents, decocher le reste,
   supprimer un critere entier si besoin
5. Cliquer sur **Appliquer** : les criteres sont injectes dans le **Query
   Builder**, ou vous les affinez comme des criteres saisis a la main

> ⚠️ **Le brouillon est une proposition, pas une definition validee.** Un LLM
> peut omettre un critere ou proposer des codes trop larges. La revue de
> l'etape 4 fait partie du processus — c'est vous qui validez la cohorte.

**Pre-requis** (sinon les criteres sortent sans codes) :

| Pre-requis | Pourquoi |
|---|---|
| **Source Value Cache peuple** pour le CDM | C'est la source de l'index semantique qui resout les termes en codes |
| **Module SapBERT actif** | Il produit les embeddings de la recherche semantique ; s'il est off, les criteres sont extraits mais les concept-sets **ne sont pas pre-remplis** |

**Ou vont mes donnees ?** Le navigateur ne joint jamais le service LLM
directement : tout passe par le backend OPAL, authentifie. En mode `embedded`,
le modele tourne dans un conteneur local et rien ne sort de l'installation. En
mode `on-premise`, le texte du prompt et les libelles partent vers **l'endpoint
LLM de votre etablissement** — celui configure dans Reglages.

> Details des modes, installation et configuration : [COHORT_LLM.md](COHORT_LLM.md).
> Cas particulier de la resolution des **medicaments** (terme ou classe →
> molecules → famille ATC) : [COHORT_LLM_MEDICAMENTS.md](COHORT_LLM_MEDICAMENTS.md).

### Concept Sets

**Acces** : onglet *Cohort Builder → Concept Sets*

Un concept set est un **ensemble de codes reutilisable** dans plusieurs cohortes.
OPAL en gere deux natures, dans un meme objet :

| Nature | Contenu | Utilisation en cohorte |
|---|---|---|
| **Concepts OMOP** | `concept_id` standard | Critere qui matche sur `concept_id` |
| **Codes source** | `source_value` bruts du CDM | Critere qui matche sur `source_value` |

Creer un concept set :

1. Cliquer sur **Create Concept Set**, nommer l'ensemble et choisir un domaine
2. Chercher puis selectionner soit des **concepts**, soit des **codes source**
   (les deux peuvent cohabiter dans le meme set)
3. Enregistrer

Utilisation :

- Depuis le Query Builder, ajouter un concept set comme critere — le type de
  critere genere depend de la nature du set (voir tableau ci-dessus)
- **Resolve** : deplie l'ensemble avec ses concepts descendants
- **Counts** : comptages de lignes et de personnes par concept sur le CDM

> Les concept sets crees sont proposes immediatement dans le Query Builder :
> la liste se rafraichit sans rechargement de page.

### Comparaison de cohortes

L'onglet **Comparer** permet de comparer deux cohortes sauvegardees :

1. Selectionner la **Cohorte A** et la **Cohorte B**
2. Cliquer sur **Comparer**
3. Les resultats montrent :
   - Comparaison demographique avec **SMD** (Standardized Mean Difference)
   - SMD > 0.1 indique un desequilibre significatif
   - Comparaison de prevalence par domaine
   - Comparaison des mesures biologiques

### Editeur SQL

L'onglet **SQL** permet d'executer des requetes SQL en lecture seule :

1. Saisir votre requete SQL (SELECT, WITH, EXPLAIN uniquement)
2. Appuyer sur **Ctrl+Entree** ou cliquer sur **Executer**
3. Les resultats s'affichent dans un tableau
4. Cliquer sur **Exporter CSV** pour telecharger les resultats

> **Note** : Seules les requetes en lecture sont autorisees. Les INSERT, UPDATE, DELETE sont bloques.

### Exporter

- **Export CSV** : liste des `person_id` avec demographie
- **Export SQL** : requete SQL generee par les criteres
- **Export direct** : export CSV sans sauvegarder la cohorte

### Parcours patient (Patient Journey)

Depuis l'echantillon de patients, cliquer sur un `person_id` ouvre sa **frise
clinique complete** : tous les evenements de tous les domaines OMOP, dates et
`source_value` compris, groupes par domaine puis par date.

Les domaines se filtrent individuellement (clic sur la legende) pour isoler,
par exemple, la seule sequence medicamenteuse.

> Fonction d'inspection qualitative : elle sert a comprendre *pourquoi* un
> patient entre dans la cohorte, pas a produire un resultat agrege.

### Pathways — parcours de traitement

**Acces** : onglet *Analyse → Pathways*

Visualisation façon **ATLAS** des sequences de traitement : quel traitement en
premier, lequel ensuite, dans quel ordre — rendu en **sunburst interactif**.

1. Definir la cohorte cible (les criteres du Query Builder)
2. Declarer de **1 a 20 cohortes d'evenements** : chacune porte un nom, un
   domaine, et des `concept_id` (avec ou sans descendants) et/ou des codes source
3. Regler les parametres :

| Parametre | Defaut | Plage | Role |
|---|---|---|---|
| **Profondeur max** | 5 | 1–10 | Nombre d'etapes successives analysees |
| **Effectif minimal par cellule** | 5 | 1–1000 | Masque les sequences trop rares (**protection de la confidentialite**) |
| **Fenetre de combinaison** | 0 | 0–365 j | Deux evenements dans cette fenetre sont traites comme une **combinaison** plutot qu'une sequence |

4. Lancer l'analyse (tache de fond, annulable, avec suivi de progression)
5. **Enregistrer** le resultat sur la cohorte : il est stocke sur sa derniere
   version et rechargeable sans recalcul

> L'effectif minimal par cellule est un garde-fou : les sequences sous le seuil
> sont supprimees du rendu et ne sont pas exportees.

### Incidence

**Acces** : onglet *Analyse → Incidence*

Calcule un **taux d'incidence** : combien de nouveaux cas d'un evenement
surviennent dans une population, rapporte au temps reellement passe a risque.

1. Choisir la **cohorte cible** (population a risque)
2. Choisir la **cohorte outcome** (l'evenement compte)
3. Definir la **fenetre a risque** (*time at risk*) :
   - debut : decalage en jours apres l'entree dans la cohorte
   - fin : **fin de la periode d'observation**, ou **duree fixe** en jours
4. Optionnel — **fenetre de nettoyage** (*clean window*) pour exclure les cas
   deja survenus avant l'entree
5. Optionnel — **stratification** par sexe et/ou tranches d'age
6. Cliquer sur **Compute**

Resultats : effectif a risque, nombre de cas, **personnes-annees**, et le taux
**pour 1000 personnes-annees** avec son **intervalle de confiance de Poisson**.
Les resultats stratifies s'affichent en tableau et en graphique.

**Save** enregistre l'analyse (parametres + resultats) pour la retrouver plus tard.

### Estimation — survie de Kaplan-Meier

**Acces** : onglet *Analyse → Estimation*

Estime la **probabilite de survie sans evenement** au cours du temps.

1. Choisir la **cohorte cible** et la **cohorte outcome**
2. Regler la fenetre a risque, l'**unite de temps** (jours par defaut) et le
   **niveau de confiance** (0.95 par defaut)
3. Optionnel — **stratification** : une courbe par strate
4. Lancer le calcul

Resultats : effectif **N**, nombre d'**evenements**, nombre de **censures**,
**survie mediane**, et la **courbe de Kaplan-Meier** avec son intervalle de
confiance (la mediane est marquee a S(t) = 0,5).

> Definitions et hypotheses statistiques (censure, personnes-annees, IC) :
> [METHODOLOGIE.md](METHODOLOGIE.md).



---

## 6. Workflow de Mapping

**Acces** : Admin, OMOP DIM, Medecin

Le workflow de mapping guide le processus de correspondance entre les codes source de votre CDM et les concepts standard OMOP.

### Vue d'ensemble

Le processus se deroule via **6 onglets** :

```
Dashboard → Unmapped → Suggestions → Manual → History → Apply Log
```

| Onglet | Role |
|---|---|
| **Dashboard** | Taux de mapping par domaine, evolution dans le temps |
| **Unmapped** | Exploration des valeurs source non mappees |
| **Suggestions** | Suggestions automatiques (SapBERT + 3 strategies internes) a valider |
| **Manual** | Mapping manuel d'une valeur source vers un concept choisi a la main |
| **History** | Historique des decisions, consensus, export STCM, marquage « deja synchronise » |
| **Apply Log** | Journal des batches appliques au CDM, avec detail et **rollback** |

### Onglet 1 : Dashboard

Vue d'ensemble des taux de mapping :

- **Graphique a barres** : taux de mapping par domaine (termes et lignes)
- **Volume non mappe** : poids en nombre de records des termes non mappes
- **Evolution** : courbe montrant l'amelioration du mapping au fil du temps
- **Performance des strategies** : taux d'approbation/modification/rejet par strategie de suggestion

### Onglet 2 : Exploration des non mappes

1. Selectionner un **domaine** (Condition, Drug, Procedure, etc.)
2. La liste des termes non mappes s'affiche :
   - **Code source** (`source_value`) : le code dans votre systeme
   - **Description** (`source_name`) : le libelle du code (si disponible)
   - **Records** : nombre de lignes concernees
   - **Personnes** : nombre de patients concernes
3. Utiliser la **barre de recherche** pour filtrer
4. **Exporter** la liste complete en CSV

> Les termes sont tries par nombre de records decroissant (les plus impactants en premier).

### Workflow collaboratif

Le mapping dans OPAL est **individuel par utilisateur** : chaque utilisateur travaille independamment sur les suggestions et prend ses propres decisions. Les termes deja approuves par un autre utilisateur restent visibles dans vos suggestions.

Un mapping n'est considere **valide** que lorsqu'il atteint le **consensus** : 2 utilisateurs ou plus ont approuve le meme mapping (meme source_value vers le meme concept cible). Tant qu'un seul utilisateur a approuve, la decision est en statut **pending**.

### Onglet 3 : Suggestions

#### Configurer les strategies

Avant de generer des suggestions, configurez les strategies activees :

- **Fuzzy** : recherche par similarite textuelle (trigrammes)
- **Keyword** : recherche par mots-cles progressifs
- **Contextual** : analyse des patterns existants dans le CDM
- **SapBERT** : suggestions pre-calculees par embeddings semantiques (si chargees)

Definir le **nombre de termes** a traiter par lot (5 a 100).

#### Generer des suggestions

1. Cliquer sur **Generer les suggestions**
2. Seuls les termes que **vous** n'avez pas encore traites apparaissent (les decisions des autres utilisateurs n'impactent pas votre liste)
3. Pour chaque terme non mappe, une carte affiche :
   - Le **code source** et sa description
   - Les **suggestions classees** par confiance decroissante :
     - Confiance en vert (≥80%) : forte probabilite
     - Confiance en orange (50-79%) : a verifier
     - Confiance en rouge (<50%) : faible probabilite
   - La **source** de la suggestion (SapBERT, Exact, Fuzzy, etc.)
   - Le **concept cible** propose (nom, code, vocabulaire)

#### Valider les suggestions

Pour chaque terme, vous pouvez :

- **Approuver** (pouce vert) : accepter la suggestion proposee
- **Modifier** (crayon) : choisir un concept cible different
- **Rejeter** (croix rouge) : marquer comme "pas de mapping applicable" (le terme reapparaitra dans les suggestions)

Vous pouvez aussi ajouter une **raison** pour documenter votre decision.

#### Approbation en lot

Pour accelerer le processus :
- **Approuver tout ≥90%** : approuve automatiquement les suggestions avec une confiance >= 90%
- **Approuver tout ≥80%** : seuil plus bas pour les suggestions ≥ 80%

### Onglet 4 : Historique et Application

#### Vue groupee

L'historique presente les decisions de **tous les utilisateurs**, groupees intelligemment :

- Les lignes sont triees par **source value** (les decisions sur le meme terme se suivent)
- Si 2 utilisateurs approuvent le meme mapping (meme source → meme cible), ils sont **fusionnes sur une seule ligne** avec les deux noms d'utilisateurs et un compteur
- La colonne **Source** affiche le libelle (`source_name`) quand il existe, avec le code en petit

#### Indicateurs de statut

| Icone | Statut | Signification |
|-------|--------|---------------|
| Tick vert | **Consensus** | 2+ utilisateurs ont approuve le meme mapping |
| Triangle jaune | **Conflit** | Des utilisateurs ont mappe le meme terme vers des cibles differentes |
| *(vide)* | **Single** | Un seul utilisateur a pris une decision |

#### Tag d'action

| Tag | Couleur | Signification |
|-----|---------|---------------|
| **pending** | Orange | Approuve par un seul utilisateur, en attente de validation |
| **approved** | Vert | Consensus atteint (2+ utilisateurs) |
| **rejected** | Rouge | Mapping rejete |
| **rolled_back** | Gris | Decision annulee |

#### Actions directes depuis l'historique

Pour chaque ligne, des boutons d'action sont disponibles selon votre relation avec la decision :

- **Tick vert** (Approuver) : approuver ce mapping pour vous — visible si vous n'avez pas encore vote sur cette ligne et qu'un concept cible existe
- **Croix rouge** (Rejeter) : rejeter ce mapping en place (passe la decision en "rejected") — visible si c'est votre decision ou si vous etes admin
- **Fleche retour** (Retirer) : retirer silencieusement votre decision (pas de trace) — visible si c'est votre decision

> **Astuce** : pour resoudre un conflit (triangle jaune), approuvez la bonne ligne et rejetez l'autre directement depuis l'historique.

#### Filtres

- **Domaine** : filtrer par domaine clinique
- **Action** : filtrer par type de decision (approved, rejected, etc.)
- **Utilisateur** : filtrer par utilisateur (liste dynamique)

#### Appliquer les mappings

Seuls les mappings ayant atteint le **consensus** (2+ utilisateurs d'accord) sont inclus dans l'application et l'export :

1. Selectionner un **domaine**
2. Cliquer sur **Preview** pour voir l'impact :
   - Nombre total de decisions consensus
   - Nombre de lignes impactees
   - Nombre de personnes impactees
3. **Exporter STCM CSV** (recommande) : telecharge un fichier CSV au format `source_to_concept_map` contenant uniquement les mappings consensus

> **Note** : Les decisions en statut "pending" (un seul utilisateur) ne sont pas incluses dans l'export. Demandez a un collegue de valider vos mappings pour qu'ils atteignent le consensus.

### Onglet 5 : Manual

Quand aucune suggestion ne convient, l'onglet **Manual** permet de mapper une
valeur source a la main : chercher le concept cible (par nom, code ou ID), le
selectionner, enregistrer. La decision suit le meme workflow de consensus que
les decisions issues des suggestions.

### Onglet 6 : Apply Log

Journal des **batches d'application**. Un batch regroupe toutes les lignes
ecrites lors d'une meme operation d'application.

1. Selectionner un batch pour voir son **detail valeur source par valeur source**
   (concept precedent → concept applique, nombre de lignes impactees)
2. Bouton **Rollback** : restaure les `concept_id` precedents enregistres dans le
   journal, puis marque le batch comme annule

> Le rollback restaure les valeurs originales dans la table clinique du CDM.
> L'operation est tracee dans le journal d'audit.

### Marquer « deja synchronise »

Certaines decisions correspondent a un mapping **deja present dans le CDM**. Pour
ne pas les re-appliquer, l'onglet History propose de les marquer `synced` :

- Une icone base de donnees signale les decisions **synchronisables** (la cible
  est deja le seul concept mappe pour cette valeur source dans le CDM)
- Bouton **Marquer synced** sur la selection ; l'operation est **reversible**
  (unmark)
- La detection se fait depuis le **cache de source values**, sans requeter le CDM

---

## 7. Charger un referentiel (CCAM, CIM-10...)

**Acces** : Admin, OMOP DIM

Les referentiels (« codebooks ») fournissent les **libelles** des codes source.
Sans eux, un code CCAM comme `QCJA003` reste sans description : la recherche par
mot-cle en francais ne trouve rien et les suggestions de mapping sont degradees.

> ⚠️ **Il n'existe pas d'ecran pour cet upload.** Il se fait par l'API ou par le
> script fourni. C'est une operation d'administration, faite une fois par
> referentiel (et rejouee a chaque mise a jour de la nomenclature).

> **OPAL ne fournit pas les fichiers** (licences, mises a jour) : vous les
> fournissez. Sources typiques : ATIH pour la CCAM et la CIM-10 FR.

### Referentiels typiques

| Referentiel | Domaine OMOP | Effet |
|---|---|---|
| CCAM FR | `Procedure` | Libelles FR des actes (recherche par mot-cle + mapping) |
| CIM-10 FR | `Condition` | Libelles FR des diagnostics |
| ATC | `Drug` | Libelles de classes medicamenteuses |

### Format du CSV

**2 colonnes minimum** : un code, une description. Le reste est auto-detecte :

| Element | Detection |
|---|---|
| Delimiteur | `,` ou `;` — celui qui apparait le plus dans la 1re ligne |
| Encodage | UTF-8 (BOM supporte), repli automatique en Latin-1 |
| Colonne **code** | En-tete parmi `ccam`, `code_ccam`, `code_cim`, `cim`, `code_acte`, `code` — sinon **1re colonne** |
| Colonne **description** | En-tete parmi `description`, `libelle`, `libellé`, `label`, `nom`, `designation`, `désignation` — sinon **2e colonne** |

Exemple minimal (`ccam_fr.csv`) :

```csv
code;libelle
QCJA003;Exérèse d'une lésion cutanée
HBFA003;Appendicectomie
```

Les lignes dont le code **ou** la description est vide sont ignorees, et les
**doublons de code sont dedupliques** (la premiere occurrence gagne).

### Methode 1 — script `reload_codebooks.sh` *(recommande)*

```bash
OPAL_USER='admin' OPAL_PASSWORD='<mot-de-passe>' KEYCLOAK_CLIENT_ID='opal-cli' \
  ./scripts/reload_codebooks.sh \
  --referentiel /chemin/vers/ccam_fr.csv \
  --domaine Procedure \
  --nom CCAM_FR
```

> **Piege d'authentification** : le script exige le client Keycloak `opal-cli`
> (Direct Access Grants). Si vous avez fait `source .env`, `KEYCLOAK_CLIENT_ID`
> vaut `opal-frontend`, qui ne les a pas — d'ou l'obligation de le **forcer** sur
> la ligne de commande. Alternative : fournir un jeton deja obtenu via `AUTH_TOKEN`.

### Methode 2 — appel API direct

```bash
curl -s -X POST "http://<host>:8000/api/mapping/reference/upload" \
  -H "Authorization: Bearer $TOKEN" \
  -F "name=CCAM_FR" -F "domain=Procedure" -F "file=@ccam_fr.csv"
```

Reponse : `{ "name": "CCAM_FR", "domain": "Procedure", "count": 7834 }`

### Remplacement et suppression

- **Re-uploader avec le meme `name` remplace integralement** les lignes
  precedentes (pas de fusion) : c'est la facon de mettre a jour une nomenclature
- Lister : `GET /api/mapping/reference`
- Supprimer : `DELETE /api/mapping/reference/{name}`

### ⚠️ Ordre des operations — a respecter

```
1. Charger les referentiels      (POST /api/mapping/reference/upload)
2. Peupler le Source Value Cache (Reglages → Source Value Cache → Build)
3. (optionnel) Construire SapBERT
```

Les libelles du referentiel ne sont appliques aux valeurs source **qu'au moment
du peuplement du cache**. Si vous chargez un referentiel **apres**, il faut
**relancer le peuplement** du cache, sinon les libelles n'apparaitront pas dans
l'explorateur de concepts ni dans le constructeur de cohortes.

> Plusieurs referentiels peuvent couvrir le meme domaine (ex. `CCAM_FR` et
> `CCAM_EN`). Le libelle retenu vient du codebook **le plus riche** du domaine
> (celui qui contient le plus de codes) ; les autres servent de repli pour les
> codes qu'il ne couvre pas ou dont le libelle est vide. Le nom du fichier n'a
> aucune importance dans ce choix.

### Mappings SapBERT pre-calcules

Les mappings SapBERT alimentent les suggestions instantanees de l'onglet
Suggestions. Deux facons de les produire :

| Voie | Quand |
|---|---|
| **Dans l'application** — Reglages → Source Value Cache, case *SapBERT*, ou onglet SapBERT | Cas normal : le build reutilise le cache existant |
| **Hors ligne** — `scripts/sapbert_mapping.py` puis `POST /api/mapping/sapbert/upload` | Calcul sur une autre machine (GPU dedie), ou import d'un mapping externe |

Format CSV attendu par l'upload : `source_code, source_name, rank,
target_concept_id, target_concept_code, target_concept_name,
target_vocabulary_id, similarity`.

Gestion : `GET /api/mapping/sapbert` (liste), `DELETE /api/mapping/sapbert/{domain}`.

---

## 8. Data Management — extraire un jeu de donnees

**Acces** : Admin, OMOP DIM

Cette page produit un **export de donnees patient** a partir d'une cohorte
sauvegardee : vous choisissez les tables et les colonnes, OPAL genere un **ZIP
contenant un CSV par table**, restreint aux patients de la cohorte.

### Etape 1 — Choisir une cohorte

Selectionner une cohorte sauvegardee du CDM courant (son effectif est affiche).
C'est elle qui definit la population extraite.

### Etape 2 — Choisir tables et colonnes

1. Filtrer la liste des tables OMOP disponibles
2. Cocher les tables voulues, puis, table par table, les **colonnes**
3. Des **presets** permettent de tout selectionner ou de tout vider

### Etape 3 — Options et lancement

| Option | Effet |
|---|---|
| **Same Visit Only** | Ne garde que les enregistrements rattaches a la **meme visite** que celle qui a qualifie le patient |
| **Apercu du schema** | Affiche les colonnes du dataset resultant, **sans extraire de donnees** — a utiliser pour verifier la selection avant de lancer |

> ⚠️ **Same Visit Only** n'est disponible que si la cohorte a ete **construite
> avec l'option « meme visite »**. Sur une autre cohorte, l'extraction est
> refusee avec une erreur explicite : il faut recreer la cohorte avec cette
> option.

Lancer l'extraction : elle tourne **en tache de fond**, avec progression
(`table en cours`, `n/total`). Vous pouvez quitter la page et revenir — l'UI se
rattache a la tache en cours.

### Etape 4 — Recuperer le resultat

Une fois la tache terminee, **telecharger le ZIP** (un CSV par table
selectionnee). Une extraction en cours peut etre **annulee** a tout moment.

> **Limites** : le nombre d'extractions simultanees est plafonne (au-dela, la
> demande est refusee avec un `429`) et le lancement est limite a 3 par minute.
> Une tache ne peut etre consultee et telechargee que par **l'utilisateur qui
> l'a lancee**.

> **Donnees identifiantes** : cet export sort des donnees patient de la
> plateforme. Les CSV produits relevent de la politique de gouvernance de votre
> etablissement — voir [MATRICE_HABILITATION.md](MATRICE_HABILITATION.md).

---

## 9. Lineage — lignage ETL

**Acces** : lecture pour tous les utilisateurs ayant acces au CDM ; upload
reserve a Admin / OMOP DIM

La page **Lineage** repond a la question « d'ou vient cette donnee ? » : elle
affiche le chemin des donnees depuis les systemes sources jusqu'aux tables OMOP.

### Charger une documentation ETL

1. Cliquer sur **Upload ETL Doc (.html)**
2. Choisir le fichier **HTML** de documentation ETL
3. OPAL le parse en graphe (tables source, tables cibles, transformations)

> Le format attendu est **HTML**. Un nouvel upload **remplace** le lignage
> precedemment enregistre pour ce CDM.

### Lire le diagramme

Le graphe est organise en **trois couches** :

| Couche | Contenu |
|---|---|
| **Sources** | Systemes et tables sources |
| **Staging** | Tables intermediaires de transformation |
| **OMOP CDM** | Tables OMOP finales |

- **Rechercher** une table par son nom
- **Zoomer / recadrer** et deplacer le graphe
- Cliquer sur une table OMOP pour ne garder que les **chaines qui y aboutissent**
  (remontee de proche en proche jusqu'aux sources)
- Un bandeau resume le nombre de **tables**, de **flux** et de **systemes sources**

Le lignage enregistre peut etre **supprime** pour repartir de zero.

---

## 10. Explorateur de Concepts

**Acces** : Admin, OMOP DIM, Chercheur, Medecin

L'explorateur permet de naviguer dans le vocabulaire OMOP et de comprendre les correspondances entre codes source et concepts standard.

### Recherche par concept

1. Selectionner l'onglet **Par concept**
2. Saisir un terme de recherche (nom, code ou ID numerique)
3. Utiliser les filtres optionnels :
   - **Domaine** : Condition, Drug, Procedure, etc.
   - **Vocabulaire** : SNOMED, ICD10, ATC, etc.
   - **Standard uniquement** : n'afficher que les concepts standard
4. Les resultats s'affichent dans un tableau :
   - ID, Nom, Code, Domaine, Vocabulaire, Classe, Flag standard
   - Nombre de records et de personnes (charge a la demande)

### Recherche par code source

1. Selectionner l'onglet **Par code source**
2. Saisir un code source ou un libelle (ex: `HYDROXYZINE`, `E11.9`)
3. Filtrer par domaine si necessaire
4. Les resultats montrent :
   - Domaine, code source, description
   - Concept mappe (si existant) avec lien
   - Nombre de records et personnes

### Detail d'un concept

Cliquer sur un concept pour afficher le panneau de detail :

#### Onglet Info
- **Metadonnees** : ID, nom, code, domaine, vocabulaire, classe, dates de validite
- **Synonymes** : noms alternatifs du concept

#### Onglet Relations
- **Concepts lies** : tous les concepts en relation (Maps to, Is a, Has finding site, etc.)
- Type de relation, concept lie, vocabulaire

#### Onglet Hierarchie
- **Ancetres** : concepts parents dans la hierarchie (avec niveaux de separation)
- **Descendants** : concepts enfants (avec niveaux de separation)
- Permet de comprendre la position d'un concept dans la taxonomie OMOP

#### Onglet Codes source
- **Codes source mappes** : tous les codes source dans vos tables cliniques qui pointent vers ce concept
- Nombre de records et personnes par code source
- Utile pour comprendre quels codes locaux correspondent a un concept standard

---

## 11. Outils OHDSI

**Acces** : Admin, OMOP DIM

Cette page permet de lancer des outils de l'ecosysteme OHDSI directement depuis OPAL.

### Services disponibles

| Service | Description | Duree typique |
|---------|-------------|---------------|
| **Achilles** | Caracterisation complete du CDM | 10-60 min |
| **Achilles Export** | Export des resultats Achilles | 5-15 min |
| **DQD** | Data Quality Dashboard | 15-45 min |
| **CDM Onboarding** | Rapport d'embarquement | 5-20 min |

### Utilisation

1. Configurer les parametres (panneau gauche) :
   - **Schema resultats** : schema pour stocker les resultats (ex: `omop_cdm`)
   - **Schema vocabulaire** : schema contenant les tables vocabulaire
   - **Version CDM** : version du modele (ex: `5.4`)
   - **Nom source** : nom descriptif du CDM
2. Cliquer sur **Lancer** pour le service souhaite
3. Le statut passe de "Inactif" a "En cours" (tag orange)
4. Les **logs s'affichent en temps reel** dans le terminal en bas de page
5. A la fin, le statut passe a "Termine" (vert) ou "Erreur" (rouge)
6. Consulter les fichiers de sortie dans le **navigateur de fichiers**

### Navigateur de fichiers

- Naviguez dans les dossiers de sortie via les breadcrumbs
- Cliquez sur un fichier pour le telecharger
- La taille du fichier est affichee

---

## 12. Parametres

**Acces** : Admin, OMOP DIM

Configurez les parametres d'analyse pour chaque CDM.

### Parametres disponibles

| Parametre | Defaut | Plage | Description |
|-----------|--------|-------|-------------|
| **Schema OMOP** | `omop_cdm` | texte | Nom du schema PostgreSQL contenant les tables OMOP |
| **Top termes non mappes** | 50 | 1-500 | Nombre de termes non mappes affiches dans les analyses |
| **Top concepts** | 50 | 1-500 | Nombre de top concepts affiches dans les analyses |
| **Max records/personne** | 100 | 10-1000 | Seuil pour la distribution records par personne |
| **Max mois observation** | 120 | 12-600 | Cap pour l'histogramme de duree d'observation |
| **Seuil alerte comparaison** | 5.0% | 0.1-50 | Pourcentage d'ecart declenchant une alerte |

### Modifier les parametres

1. Selectionner le CDM concerne dans le selecteur
2. Modifier les valeurs souhaitees
3. Cliquer sur **Sauvegarder**

### Source Value Cache

Le **cache de valeurs source** pre-calcule, par CDM et par domaine, la liste des
`source_value` distincts avec leurs comptages. Il est utilise par la recherche
de concepts, l'autocompletion du constructeur de cohortes, l'explorateur de
mapping et les exports CSV.

**Sans cache, ces ecrans interrogent le CDM en direct et sont beaucoup plus
lents.** C'est aussi le cache qui alimente l'index de l'assistant IA.

| Action | Effet |
|---|---|
| **Build Cache** | Lance le peuplement (asynchrone, avec progression par domaine) |
| **Refresh** | Rafraichit l'affichage de l'etat |
| **Cancel** | Annule le peuplement en cours |
| **Clear** | Vide le cache (tout le CDM, ou un domaine) |

Chaque domaine est **commite independamment** : un domaine `done` est deja
exploitable pendant que les autres tournent encore.

> ⚠️ **Ordre a respecter** : charger les referentiels **avant** de peupler le
> cache (voir *Charger un referentiel*). Les libelles ne sont appliques qu'au
> moment du peuplement ; charges apres, il faut relancer le Build.

### Suggestions semantiques SapBERT

Depuis la meme carte, la case **Build SapBERT semantic suggestions** enchaine, apres
le peuplement, la construction des suggestions semantiques multilingues pour les
domaines **qui ont des libelles** (referentiels charges ou `source_name` du CDM).

- Un build SapBERT peut aussi etre lance **seul** : il **reutilise le cache
  existant**, sans le reconstruire
- **Choisir les domaines** a construire : le domaine `Drug` est notablement plus
  long, on peut le decocher
- Le build est **annulable** — l'arret prend effet **apres le domaine en cours**
- Par (CDM, domaine), un interrupteur active ou desactive l'usage des
  suggestions SapBERT ; le reglage **survit aux reconstructions**

> Quand SapBERT est desactive, les autres strategies de suggestion de mapping
> continuent de fonctionner normalement.

### LLM Cohorte (on-premise)

**Reservee aux admins**, cette carte n'apparait qu'en mode `on-premise` :

| Champ | Exemple | Obligatoire |
|---|---|---|
| **URL (base OpenAI-compatible)** | `https://llm.chu.fr/v1` | ✅ |
| **Modele** | `llama3.1:70b-instruct` | ✅ |
| **Cle API** | `sk-…` | ❌ — seulement si votre endpoint exige une authentification |

> La cle est **chiffree** en base et **jamais reaffichee** : le champ reste vide,
> un indicateur signale qu'une cle est enregistree. Pour la changer, retapez une
> valeur et enregistrez.

**Construire l'index RAG** reconstruit l'index semantique de l'assistant IA a
partir du Source Value Cache du CDM selectionne. A relancer apres chaque mise a
jour du cache.

---

## 13. Audit et Administration

### Journal d'audit

**Acces** : Admin

Le journal d'audit trace toutes les actions effectuees dans OPAL.

#### Consulter les logs

1. Acceder a la page **Audit**
2. Les statistiques du jour s'affichent en haut :
   - Total des evenements
   - Nombre d'utilisateurs actifs
   - Repartition par type d'action
3. Utiliser les filtres :
   - **Plage de dates** : selectionner une periode
   - **Utilisateur** : filtrer par nom d'utilisateur
   - **Action** : filtrer par type (quality, cohort, mapping, cdm, concept, ohdsi)
4. Le tableau affiche : heure, utilisateur, action, methode HTTP, chemin, statut, duree, IP
5. Les codes de statut sont colores : vert (2xx), bleu (3xx), orange (4xx), rouge (5xx)
6. **Exporter** les logs en CSV

### Gestion des utilisateurs

**Acces** : Admin

#### Onglet Utilisateurs

- Liste de tous les utilisateurs Keycloak
- Pour chaque utilisateur :
  - **Roles** : affiches comme des tags colores
  - **Ajouter un role** : selectionner dans le dropdown et cliquer sur ajouter
  - **Retirer un role** : cliquer sur la croix du tag
  - **Activer/Desactiver** : basculer le switch
- Cliquer sur un nom d'utilisateur pour voir le detail (ID, email, date de creation)

#### Onglet Groupes d'utilisateurs

**Acces** : Admin, OMOP DIM

Un **groupe** rassemble des utilisateurs pour leur accorder des droits en une
seule operation (au lieu d'un grant par personne).

- **Creer** un groupe : nom, description, et eventuellement des membres d'emblee
- **Modifier** : la description, ou la liste des membres (**la liste fournie
  remplace l'ensemble des membres**)
- **Ajouter / retirer** un membre individuellement
- **Supprimer** le groupe : les acces accordes via ce groupe disparaissent avec lui

### Controle d'acces par CDM

**Acces** : Admin (permission `can_manage_access`)

Independamment des roles, l'acces peut etre restreint **CDM par CDM** :

| Action | Effet |
|---|---|
| **Accorder a un utilisateur** | Cet utilisateur voit et utilise ce CDM |
| **Accorder a un groupe** | Tous les membres du groupe y ont acces |
| **Revoquer** | Retire l'acces (utilisateur ou groupe) |
| **Tout supprimer pour un CDM** | Retire **tous** les controles d'acces du CDM (permission dediee `can_clear_all_grants`) |

> **Regle importante** : un CDM **sans aucun controle d'acces** est visible par
> tous les utilisateurs autorises par leur role. Des qu'un premier acces est
> accorde, le CDM devient **restreint** : seuls les beneficiaires (directs ou via
> un groupe) y accedent.

Chaque utilisateur ne voit dans le selecteur de CDM que ceux auxquels il a acces.

### Partage de cohortes

Une cohorte appartient a son createur. Elle peut etre partagee de trois facons :

| Portee | Effet |
|---|---|
| **Utilisateur** | Une personne nommement designee y accede |
| **Groupe** | Tous les membres du groupe y accedent |
| **Tous** | Tous les utilisateurs de la plateforme y accedent |

- Le partage se gere depuis la cohorte : **partager**, **lister les partages**,
  **retirer un partage**
- Les personnes concernees recoivent une **notification**
- Seuls le **proprietaire** et les **admins** peuvent modifier les partages
- Les admins et OMOP DIM disposent d'une vue **toutes les cohortes par
  createur**, qui montre aussi ce a quoi chacun accede par partage

#### Onglet Demandes d'acces

- Badge indiquant le nombre de demandes en attente
- Pour chaque demande :
  - Nom d'utilisateur, nom complet, email, role demande
  - **Approuver** : cree automatiquement le compte Keycloak
    - Un mot de passe temporaire est genere et affiche (a copier et communiquer)
    - L'utilisateur devra le changer a sa premiere connexion
  - **Rejeter** : supprime la demande

---

## 14. FAQ et Depannage

### Questions frequentes

**Q : Pourquoi je ne vois pas certaines pages dans le menu ?**
R : Votre role ne vous donne pas acces a ces fonctionnalites. Contactez un administrateur pour modifier vos permissions.

**Q : Comment changer la langue ?**
R : Cliquez sur le bouton FR/EN dans la barre de navigation supérieure (TopNav). Le choix est retenu entre les sessions.

**Q : Les analyses sont lentes, que faire ?**
R : La duree depend de la taille de votre CDM. Pour les gros CDM (>1M patients), certaines analyses peuvent prendre plusieurs minutes. Utilisez l'analyse par lot pour lancer tous les domaines en une fois.

**Q : Puis-je utiliser OPAL avec un CDM non-PostgreSQL ?**
R : Non, OPAL ne supporte que les CDM PostgreSQL. Le connecteur utilise `psycopg2` qui est specifique a PostgreSQL.

**Q : L'ecriture dans le CDM est-elle risquee ?**
R : L'ecriture est limitee a la table `source_to_concept_map` et utilise un UPSERT transactionnel. En cas d'erreur, un rollback automatique annule toutes les modifications. Il est neanmoins recommande d'utiliser l'export STCM CSV et de l'appliquer via votre propre processus ETL.

**Q : Comment sauvegarder mes donnees ?**
R : Les donnees OPAL sont dans la base PostgreSQL `opal-db`. Utilisez `pg_dump` pour les sauvegardes. N'oubliez pas de sauvegarder aussi le fichier `.secret_key` (necessaire pour dechiffrer les mots de passe CDM).

**Q : Comment je pousse un referentiel CCAM pour les procedures ?**
R : Par l'API ou le script `scripts/reload_codebooks.sh` — **il n'y a pas
d'ecran pour cela**. Le CSV a besoin de 2 colonnes (code, libelle) ; le domaine
vise est `Procedure`. Attention a l'ordre : charger le referentiel **avant** de
peupler le Source Value Cache. Pas-a-pas complet : section
[7. Charger un referentiel](#7-charger-un-referentiel-ccam-cim-10).

**Q : Comment fonctionne le cohorting par LLM ?**
R : Vous decrivez la cohorte en francais dans l'onglet *Assistant IA* ; un LLM
extrait les criteres, puis une recherche semantique (RAG) les fait correspondre
aux **codes reels de votre CDM**. Vous revoyez les codes proposes avant de les
appliquer au Query Builder. La fonctionnalite est **desactivee par defaut**, et
requiert un Source Value Cache peuple. Voir
[5. Assistant IA](#5-constructeur-de-cohortes) et [COHORT_LLM.md](COHORT_LLM.md).

**Q : L'onglet « Assistant IA » n'apparait pas.**
R : La fonctionnalite est desactivee cote serveur (`COHORT_LLM_MODE=off`), ou
votre role n'y donne pas acces. C'est un choix d'installation : demandez a un
administrateur.

**Q : L'assistant IA me renvoie des criteres sans aucun code.**
R : Le **Source Value Cache** du CDM n'est pas peuple — c'est lui qui alimente
l'index semantique. Le construire depuis *Reglages → Source Value Cache*, puis
reconstruire l'index RAG.

**Q : J'ai charge un referentiel mais les libelles francais n'apparaissent pas.**
R : Les libelles ne sont appliques qu'**au moment du peuplement du cache**.
Relancez le Build du Source Value Cache apres avoir charge le referentiel.

**Q : Mon vocabulaire OMOP est dans un autre schema que les donnees cliniques.**
R : C'est prevu : utilisez les **schemas par categorie** dans la configuration
du CDM (section [3. Gestion des CDM](#3-gestion-des-cdm)) et pointez la
categorie `vocabulary` vers le schema partage.

**Q : Comment donner acces a un CDM a toute une equipe ?**
R : Creez un **groupe**, ajoutez-y les membres, puis accordez l'acces au groupe
plutot qu'a chaque personne (section
[13. Audit et Administration](#13-audit-et-administration)).

**Q : Puis-je extraire les donnees d'une cohorte en CSV ?**
R : Oui, via la page **Data Management** : choix de la cohorte, des tables et
des colonnes, puis telechargement d'un ZIP contenant un CSV par table
(section [8. Data Management](#8-data-management--extraire-un-jeu-de-donnees)).

**Q : J'ai applique un mapping par erreur, puis-je revenir en arriere ?**
R : Oui. L'onglet **Apply Log** du workflow de mapping liste les batches
appliques et permet un **rollback** qui restaure les `concept_id` precedents.

### Depannage

**Probleme : "CDM connection failed" (502)**
- Verifiez que le serveur PostgreSQL du CDM est accessible
- Verifiez les identifiants de connexion
- Verifiez que le port est ouvert dans le pare-feu
- Utilisez le bouton "Tester" dans la page Gestion des CDM

**Probleme : "Missing or invalid Authorization header" (401)**
- Votre session a expire, rechargez la page
- Si le probleme persiste, deconnectez-vous et reconnectez-vous

**Probleme : "Forbidden: insufficient permissions" (403)**
- Votre role ne permet pas cette action
- Contactez un administrateur pour obtenir les permissions necessaires

**Probleme : L'analyse batch reste bloquee**
- Verifiez la connexion au CDM
- Essayez une analyse sur un seul domaine pour isoler le probleme
- Consultez les logs du backend (`docker compose logs opal-backend`)

**Probleme : Les suggestions de mapping sont vides**
- Verifiez que l'extension `pg_trgm` est installee sur le CDM (requise pour la recherche fuzzy)
- Chargez des codebooks de reference pour enrichir les descriptions
- Chargez des mappings SapBERT pour des suggestions instantanees

**Probleme : Le parcours patient ne s'affiche pas**
- Verifiez que le `person_id` existe dans le CDM
- Le patient doit avoir des evenements cliniques dans au moins un domaine

### Support

Pour obtenir de l'aide :
- Consultez la [documentation API](API.md) pour les details techniques
- Consultez la [documentation technique](TECHNICAL.md) pour l'architecture
- Contactez l'equipe OPAL de l'AP-HM
