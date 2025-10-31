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
| Docker & Docker Compose | Pour la mise en place de l’environnement | 28.5.1 |


## Source de données / API utilisée
Nous avions un choix d'API à faire parmi plusieurs API publique et aérienne et notre choix s'est porté sur OpenAIP qui contient des données aéronautiques mondiales actuelles et précises.

## Architecture du projet 
```
   [OpenSky Network API]
        ↓
   [Apache NiFi]
        ↓
   [Kafka Topics]
        ↓
   [Spark Streaming]
        ↓
   [PostgreSQL]
        ↓
   [Grafana Dashboard]
```

## Arborescence du projet
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
- Récupération des données sur le flux du trafic aérien en temps réel via l'API "OpenSky Network" disponible en ligne puis en envoi des données dans Kafta, en passant par Apache NIFI.
  
2. **Centralisation des messages**:
- Après l'envoi des données dans Kafka, centralisation des messages produits par NIFI pour les rendre disponible pour Spark.
  
3. **Traitement des données**:
- Traitement des données dans Spark avec nettoyage et agrégation pour avoir des données propres qui seront exploitables.

4. **Stockage Distribué**:
- Sauvegarde des données dans une base consultable (PostGres).

5. **Visualisation et Analyse**:
- Grafana est utilisé pour créer des tableaux de bord interactifs, permettant de suivre l'état du trafic aérien.

## Description du pipeline
Le pipeline mis en place a pour objectif de traiter en **quasi-temps réel** les données issues de l’API **OpenSky Network**, afin de suivre et d’analyser le trafic aérien mondial.

L’ensemble de la chaîne de traitement est **conteneurisé sous Docker**, et repose sur un ensemble d’outils Big Data intégrés de manière cohérente :

---
### Ingestion avec NIFI & Envoi dans Kafka
Le pipeline débute avec **Apache NiFi**, chargé de se connecter à l’API publique **OpenSky Network**.  
Cette API permet d’obtenir des données en temps quasi réel sur le trafic aérien mondial (position, altitude, vitesse, le pays d'origine, immatriculation, etc.).  

NiFi est configuré pour :
- interroger périodiquement l’endpoint de l’API (`https://opensky-network.org/api/states/all`),
- parser la réponse JSON,
- extraire tous les champs de l'API,
- reformater les données dans une structure standardisée,
- puis les envoyer dans un **topic Kafka** via le processeur `PublishKafkaRecord_2_0`, nommer *airports*.

Cela permet d’assurer un flux continu de messages Kafka représentant les états des avions en temps réel.

<img width="1552" height="379" alt="Capture d&#39;écran 2025-10-29 134902" src="https://github.com/user-attachments/assets/6c63a507-3054-41b5-b294-d3856903eb67" />

  
### Centralisation des flux avec Kafka et envoi dans Spark
**Apache Kafka** assure la centralisation et la diffusion du flux de données aériennes.  
Le topic `airports` contient les messages JSON complets envoyés par NiFi.  
Ce système permet d’assurer la tolérance aux pannes et la scalabilité du traitement.

Pour vérifier le bon acheminement des messages :
```bash
kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic airports --from-beginning
```

### Traitement des données dans Spark 
   - Traitement des données dans Spark avec nettoyage et agrégation pour avoir des données propres qui seront exploitables.
   
### Stockage distribué dans PostgreSQL
Les données aériennes traitées sont stockées dans **PostgreSQL** dans une table `airports` contenant les champs suivants :

| **Nom du champ** | **Description** | **Exemple de valeur** |
|----------------------|--------------------|---------------------------|
| `icao24` | Identifiant unique de l’aéronef attribué par l’OACI (24 bits) | `39c1b3` |
| `callsign` | Indicatif de vol (nom du vol ou code de la compagnie) | `AFR1234` |
| `origin_country` | Pays d’origine de l’aéronef ou de la compagnie | `France` |
| `longitude` | Coordonnée géographique Est/Ouest en degrés | `2.3522` |
| `latitude` | Coordonnée géographique Nord/Sud en degrés | `48.8566` |
| `baro_altitude` | Altitude barométrique de l’avion (en mètres) | `10450.5` |
| `geo_altitude` | Altitude géométrique basée sur GPS (en mètres) | `10600.0` |
| `velocity` | Vitesse sol de l’aéronef (en m/s) | `220.5` |
| `true_track` | Cap vrai suivi par l’aéronef (en degrés) | `90.0` |
| `vertical_rate` | Vitesse verticale (m/s), positive en montée, négative en descente | `-5.2` |
| `on_ground` | Indique si l’aéronef est au sol (`true`) ou en vol (`false`) | `false` |
| `spi` | Special Position Identification (alerte spécifique)| `false` |
| `squawk` | Code transpondeur à 4 chiffres (identifiant temporaire du transpondeur) | `4713` |
| `position_source` | Source de la position | `0` |

     
## Visualisation des données avec Grafana
**Grafana** se connecte à PostgreSQL pour visualiser les indicateurs en quasi temps réel :
- Carte du trafic aérien (positions longitude/latitude),
- Altitude et vitesse moyenne par avion ou par zone géographique,
- Distribution des vols par pays d’origine,
- Nombre d’avions en vol / au sol.
- Nombre d'avion en situation spécifique (alerte)

Les dashboards sont mis à jour dynamiquement grâce au rafraîchissement automatique de Grafana.

<img width="1331" height="685" alt="Capture d&#39;écran 2025-10-31 093127" src="https://github.com/user-attachments/assets/d53e57b0-5e79-44b6-8272-6f74055937e5" />


## Difficultés rencontrées et solutions apportées

Au cours du développement du projet, plusieurs difficultés techniques ont été rencontrées, principalement liées à l’intégration et à la compatibilité des différents outils Big Data au sein de l’environnement Dockerisé.

### 1. Problèmes de dépendances et de versions
L’une des premières difficultés concernait les **incompatibilités de versions** entre certains services (notamment entre **NiFi**, **Kafka**, **Spark** et leurs **drivers JDBC**).  
Ces différences de versions provoquaient des erreurs lors du démarrage des conteneurs ou lors des connexions inter-services (ex. Spark ↔ PostgreSQL).

**Solution :**  
Un travail d’ajustement des versions dans le fichier `docker-compose.yml` et dans les fichiers de configuration a été nécessaire.  
Chaque composant a été harmonisé pour garantir une compatibilité entre :
- le driver Kafka utilisé par Spark ;
- la version du connecteur JDBC PostgreSQL ;
- et la configuration réseau des conteneurs.

---

### 2. Adaptation du Docker Compose à l’environnement local
Lors du déploiement, des différences de configuration liées à l’environnement local (ports réservés, ressources, réseau Docker, etc.) ont empêché certains services de se lancer correctement.

**Solution :**  
Le fichier `docker-compose.yml` a été **modifié et personnalisé** pour correspondre à la configuration de la machine :
- ajustement des **ports exposés** ;
- adaptation des **volumes et chemins de stockage** ;
- correction de certaines dépendances entre services (`depends_on`) ;
- optimisation de la mémoire allouée à **Spark** et **PostgreSQL**.

Ces ajustements ont permis d’obtenir un environnement stable et fonctionnel, capable de faire communiquer correctement tous les conteneurs.

---

### 3. Interconnexion des services
La communication entre les conteneurs (**NiFi → Kafka → Spark → PostgreSQL**) n’a pas toujours été immédiate, notamment à cause de la **latence réseau interne** ou de **problèmes de résolution DNS Docker**.

**Solution :**  
Des tests successifs de connectivité et des ajustements de configuration (`bootstrap.servers`, `JDBC URLs`, `hostname`) ont été effectués.  
Chaque service a ensuite pu se connecter correctement à ses dépendances via le **réseau Docker interne**.

---

Ces difficultés, bien qu’initialement bloquantes, ont permis de mieux comprendre :
- les interactions entre conteneurs dans un environnement distribué ;
- la gestion fine des dépendances entre technologies Big Data ;
- et les bonnes pratiques de déploiement d’un pipeline complet sous Docker.



## Conclusion <a name="conclusion"></a>
Ce projet nous a permis de mettre en œuvre une chaîne complète de traitement de données en **quasi-temps réel**, depuis la collecte jusqu’à la visualisation, en mobilisant un ensemble cohérent d’outils du Big Data.

Grâce à **Apache NiFi**, nous avons automatisé l’ingestion continue des données issues de l’API **OpenSky Network**.  
**Kafka** a joué un rôle central de courtier de messages, garantissant la fiabilité et la fluidité des échanges de données entre les différents composants.  
**Spark Structured Streaming** a permis d’assurer le **nettoyage**, la **transformation** et le **chargement** des données de vol de manière distribuée et scalable, avant leur stockage dans **PostgreSQL** pour exploitation.  
Enfin, **Grafana** a offert une interface de visualisation intuitive, permettant de suivre l’évolution du trafic aérien mondial en quasi temps réel à travers plusieurs indicateurs clés (nombre d’avions en vol, altitude moyenne, répartition par pays, etc.).

Sur le plan pédagogique, ce projet a été une excellente opportunité pour :
- comprendre le fonctionnement d’un pipeline **ETL distribué et conteneurisé** ;
- expérimenter la **gestion de flux de données en streaming** avec Kafka et Spark ;
- manipuler des **données aéronautiques réelles** ;
- et construire un **tableau de bord analytique complet** en environnement Big Data.

## Contributeurs
- Solenn COULON (@solennCoulon17): Data engineer - **solenn.coulon@supdevinci-edu.fr**
- Coline TREILLE (@Coline-T) : Data analyst - **coline.treille@supdevinci-edu.fr**

