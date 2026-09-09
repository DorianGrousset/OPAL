# Documentation OPAL — Index

Point d'entrée de la documentation d'OPAL (OMOP Platform for Analytics & Lineage).
Chaque document a un public et une portée précise ; commence par la ligne qui
correspond à ton besoin.

---

## Par public

| Tu es… | Commence par | Puis |
|---|---|---|
| **Utilisateur** (chercheur, médecin, DIM) | [USER_GUIDE.md](USER_GUIDE.md) | [METHODOLOGIE.md](METHODOLOGIE.md) |
| **Administrateur / installateur** | [../README.md](../README.md) | [MATRICE_HABILITATION.md](MATRICE_HABILITATION.md), [../SECURITY.md](../SECURITY.md) |
| **Développeur / intégrateur** | [TECHNICAL.md](TECHNICAL.md) | [API.md](API.md), [../CONTRIBUTING.md](../CONTRIBUTING.md) |
| **Exploitant en incident** | [../HOTFIX.md](../HOTFIX.md) | [USER_GUIDE.md § Dépannage](USER_GUIDE.md) |

---

## Documents de référence

| Document | Contenu |
|---|---|
| [USER_GUIDE.md](USER_GUIDE.md) | Guide utilisateur complet : toutes les pages de l'application, écran par écran |
| [API.md](API.md) | Référence exhaustive des endpoints REST + WebSocket |
| [TECHNICAL.md](TECHNICAL.md) | Architecture, modèle de données, moteurs (qualité, SQL cohortes, mapping, lineage) |
| [METHODOLOGIE.md](METHODOLOGIE.md) | Méthodes statistiques et épidémiologiques (métriques qualité, incidence, survie) |
| [MATRICE_HABILITATION.md](MATRICE_HABILITATION.md) | Matrice rôles × permissions (qui peut faire quoi) |

## Fonctionnalités à documentation dédiée

| Document | Fonctionnalité |
|---|---|
| [COHORT_LLM.md](COHORT_LLM.md) | Assistant IA de construction de cohortes (« cohorting par LLM ») : modes, installation, configuration on-premise |
| [COHORT_LLM_MEDICAMENTS.md](COHORT_LLM_MEDICAMENTS.md) | Résolution des critères **médicament** par l'assistant IA (terme/classe → molécules → famille ATC) |
| [WEBSOCKET_NOTIFICATIONS.md](WEBSOCKET_NOTIFICATIONS.md) | Notifications temps réel : protocole WebSocket, tickets SSE, reconnexion |
| [../ohdsi-tools/README.md](../ohdsi-tools/README.md) | Runner OHDSI (Achilles, DQD, CDM Onboarding…) |
| [../sapbert-tools/README.md](../sapbert-tools/README.md) | Service SapBERT : embedder médical partagé (mapping + RAG de l'assistant IA) |

## Décisions d'architecture (ADR)

| ADR | Sujet |
|---|---|
| [0001](adr/0001-ohdsi-runner-dedie.md) | Runner OHDSI dédié plutôt qu'un accès au socket Docker |

## Audits

Photographies datées de l'état du produit — utiles pour le contexte, **non
normatives** (le code fait foi) :

- [AUDIT_FONCTIONNEL.md](audits/AUDIT_FONCTIONNEL.md) — couverture fonctionnelle
- [AUDIT_SECURITE.md](audits/AUDIT_SECURITE.md) — surface de sécurité
- [AUDIT_OPTIMISATION.md](audits/AUDIT_OPTIMISATION.md) — performances

## À la racine du dépôt

| Fichier | Contenu |
|---|---|
| [../README.md](../README.md) | Installation, déploiement, bootstrap des référentiels, variables d'environnement |
| [../CONTRIBUTING.md](../CONTRIBUTING.md) | Conventions de contribution, tests, style |
| [../SECURITY.md](../SECURITY.md) | Politique de sécurité et signalement de vulnérabilité |
| [../HOTFIX.md](../HOTFIX.md) | Procédures de correctif à chaud |
| [../CHANGELOG.md](../CHANGELOG.md) | Historique des versions |

---

## Questions fréquentes → où lire

| Question | Réponse dans |
|---|---|
| Comment je pousse un référentiel CCAM pour les procédures ? | [USER_GUIDE.md § Référentiels](USER_GUIDE.md) (UI) · [../README.md § Bootstrap des référentiels](../README.md) (CLI/CI) |
| Comment fonctionne le cohorting par LLM ? | [COHORT_LLM.md](COHORT_LLM.md) · usage pas-à-pas dans [USER_GUIDE.md § Assistant IA](USER_GUIDE.md) |
| Comment brancher un CDM dont le vocabulaire est dans un autre schéma ? | [USER_GUIDE.md § Schémas par catégorie](USER_GUIDE.md) · [TECHNICAL.md § SchemaMap](TECHNICAL.md) |
| Comment extraire un jeu de données à partir d'une cohorte ? | [USER_GUIDE.md § Data Management](USER_GUIDE.md) |
| Comment donner accès à un CDM à une équipe ? | [USER_GUIDE.md § Groupes et accès CDM](USER_GUIDE.md) · [MATRICE_HABILITATION.md](MATRICE_HABILITATION.md) |
| Quelles variables d'environnement existent ? | [../.env.example](../.env.example) (référence commentée) |
