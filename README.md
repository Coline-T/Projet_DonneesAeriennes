# Projet final : Traitement en quasi-temps réel de données aériennes

## Technologies utilisées
- Apache NiFi → pour l’ingestion de données
- Apache Kafka → pour le streaming distribué
- Apache Spark (Batch ou Streaming) → pour le traitement distribué
- Postgres → pour le stockage distribué
- Grafana → pour la visualisation finale
- Docker & Docker Compose → pour la mise en place de l’environnement 

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

## Source de données / API utilisée
Nous avions un choix d'API à faire parmi plusieurs API publique et aérienne et notre choix s'est porté sur OpenAIP qui contient des données aéronautiques mondiales actuelles et précises.

## Architecture du projet 
INSERER NOTRE ARCHITECTURE !!!

## Étapes du projet

1. **Ingestion des données du trafic aérien** :
   - Récupération des données sur le flux du trafic aérien en temps réel via l'API "OpenAIP" disponible en ligne puis en envoi des données dans Kafta, en passant par Apache NIFI.
  
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
### description du pipeline
.....

## Déploiement 
....

## Visualisation des données avec Grafana
....

## Difficultés rencontrées
.....

## 📜 Conclusion <a name="conclusion"></a>
....

## Contributeurs
- Solenn COULON (@solennCoulon17): Data engineer - **solenn.coulon@supdevinci-edu.fr**
- Coline TREILLE (@Coline-T) : Data analyst - **coline.treille@supdevinci-edu.fr**

