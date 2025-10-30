# Projet final : Traitement en quasi-temps réel de données aériennes

## Contexte et Objectif du projet
Ces outils ont été utilisés pour le développement du projet sur les données aériennes, visant à analyser en quasi-temps réel les données du trafic aérien. Ce projet nous permet de mettre en place une chaîne complète en passant par l'extraction, le traitement, la transformation, et la visualisation de données aériennnes, tout en mobilisant plusieurs outils Big Data dans un 
environnement Dockerisé. 
L’objectif est de construire un pipeline ETL (Extract – Transform – Load) distribué, capable de traiter en quasi temps réel un flux de données issues d’API publiques, et pour notre cas ce sont sur les flux de données aériens.

## Objectifs pédagogiques 
- Mettre en place une chaîne ETL distribuée en environnement conteneurisé (ici Docker)
- Gérer un flux de données en quasi temps réel via Kafka et Spark
- Expérimenter le stockage et la consultation de données dans un système distribué 
- Réaliser un tableau de bord BI interactif avec des données aériennes réelles 
- Projet à mener en 1 semaine par groupe de 2 étudiants

## Technologies utilisées
| Technologie | Utilisation | Version |
|------------|-------------|-------------|
| Apache NiFi | Pour l’ingestion de données | 2.6.0 |
| Apache Kafka | Pour le streaming distribué | |
| Apache Spark | Batch ou Streaming pour le traitement distribué | 4.1.6 |
| Postgres | Pour le stockage distribué | 15.14 |
| Grafana | Pour la visualisation finale | v12.2.1 |
| Docker & Docker Compose | Pour la mise en place de l’environnement | |


## Source de données / API utilisée
Nous avions un choix d'API à faire parmi plusieurs API publique et aérienne et notre choix s'est porté sur OpenAIP qui contient des données aéronautiques mondiales actuelles et précises.

## Architecture du projet 
```
C:.
├───grafana_data
│   ├───csv
│   ├───pdf
│   ├───plugins
│   │   ├───grafana-exploretraces-app
│   │   │   ├───components
│   │   │   │   └───states
│   │   │   │       └───EmptyState
│   │   │   │           └───img
│   │   │   ├───img
│   │   │   └───utils
│   │   │       └───trace-merge
│   │   │           └───test-responses
│   │   ├───grafana-lokiexplore-app
│   │   │   └───img
│   │   │       └───icons
│   │   │           ├───dark
│   │   │           └───light
│   │   ├───grafana-metricsdrilldown-app
│   │   │   └───img
│   │   └───grafana-pyroscope-app
│   │       ├───img
│   │       ├───pages
│   │       │   └───ProfilesExplorerView
│   │       │       └───components
│   │       │           └───SceneByVariableRepeaterGrid
│   │       │               └───components
│   │       │                   └───SceneEmptyState
│   │       │                       └───ui
│   │       │                           └───img
│   │       └───shared
│   │           └───infrastructure
│   │               └───profile-metrics
│   └───png
├───nifi_data
├───postgres_data
│   ├───base
│   │   ├───1
│   │   ├───16384
│   │   ├───4
│   │   ├───5
│   │   └───pgsql_tmp
│   ├───global
│   ├───pg_commit_ts
│   ├───pg_dynshmem
│   ├───pg_logical
│   │   ├───mappings
│   │   └───snapshots
│   ├───pg_multixact
│   │   ├───members
│   │   └───offsets
│   ├───pg_notify
│   ├───pg_replslot
│   ├───pg_serial
│   ├───pg_snapshots
│   ├───pg_stat
│   ├───pg_stat_tmp
│   ├───pg_subtrans
│   ├───pg_tblspc
│   ├───pg_twophase
│   ├───pg_wal
│   │   └───archive_status
│   └───pg_xact
└───spark
```

## Étapes du projet

1. **Ingestion des données du trafic aérien** :
   - Récupération des données sur le flux du trafic aérien en temps réel via l'API "OpenSky" disponible en ligne puis en envoi des données dans Kafta, en passant par Apache NIFI.
   - believe in yourself 
  
2. **Centralisation des messages**:
   - Après l'envoi des données dans Kafka, centralisation des messages produits par NIFI pour les rendre disponible pour Spark.

3. **Traitement des données**:
   - Traitement des données dans Spark avec nettoyage et agrégation pour avoir des données propres qui seront exploitables.
   
4. **Stockage Distribué**:
   - Sauvegarde des données dans une base consultable (PostGres).
     
5. **Visualisation et Analyse**:
   - Grafana est utilisé pour créer des tableaux de bord interactifs, permettant de suivre l'état du trafic aérien.

## Déroulement technique du projet
### Ingestion avec NIFI & Envoi dans Kafka
Récupération des données et Ingestion des données dans NIFI et envoi dans Kafka (voir capture pour le schéma dans NIFI).
<img width="1552" height="379" alt="Capture d'écran 2025-10-29 134902" src="https://github.com/user-attachments/assets/d33f4567-ab01-4ba8-87c0-2a868e881d91" />

### Centralisation et envoi dans Spark
mettre une capture

### Traitement des données dans Spark 
mettre capture ?

### Description du pipeline
.....

## Visualisation des données avec Grafana
....

## Difficultés rencontrées

Problèmes de dépendances 


## 📜 Conclusion <a name="conclusion"></a>
....

## Contributeurs
- Solenn COULON (@solennCoulon17): Data engineer - **solenn.coulon@supdevinci-edu.fr**
- Coline TREILLE (@Coline-T) : Data analyst - **coline.treille@supdevinci-edu.fr**

