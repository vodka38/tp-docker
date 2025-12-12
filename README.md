TP Docker & Monitoring

Ce projet est un TP autour de Docker et du monitoring d’un environnement de conteneurs avec Prometheus et Grafana.

Objectifs

Déployer plusieurs services Docker (application + monitoring).

Collecter des métriques de conteneurs avec Prometheus et cAdvisor.

Visualiser ces métriques dans Grafana à l’aide d’un dashboard personnalisé.

Architecture

Les services principaux du TP :

Application : conteneurs applicatifs à surveiller (par ex. Nginx ou autre service fourni dans le sujet).

cAdvisor : expose les métriques des conteneurs Docker.

Prometheus : récupère les métriques de cAdvisor via HTTP.

Grafana : affiche les données de Prometheus sous forme de graphiques.

L’ensemble des services est orchestré via docker-compose.yml.

Lancement du projet

Dans le dossier du TP :

text
docker compose up -d
Services accessibles (selon la configuration fournie dans le sujet) :

Application : http://localhost:80 ou autre port défini.

Prometheus : http://localhost:9090

Grafana : http://localhost:3000

Configuration de Prometheus dans Grafana

Dans Grafana :

Aller dans Connections → Add new connection.

Choisir Prometheus.

Configurer l’URL : http://prometheus:9090

Tester la connexion avec “Save & test”.

Dashboard Grafana

Un dashboard dédié au monitoring des conteneurs a été créé avec plusieurs panels :

CPU usage

Requête : rate(container_cpu_usage_seconds_total[5m])

Affiche l’utilisation CPU par conteneur.

RAM usage

Requête : container_memory_usage_bytes

Affiche la consommation mémoire des conteneurs.

Disk I/O

Requête : rate(container_fs_io_time_seconds_total[5m])

Visualise l’activité d’entrée/sortie disque.

Container uptime

Requête : up{job="cadvisor"}

Indique si les conteneurs supervisés sont “up” ou non.

Les panels sont regroupés sur un même dashboard (par exemple “dashbord”) et sauvegardés via le bouton “Save dashboard”.

JSON du dashboard et champ “lasagna”

Le TP demande d’ajouter un champ personnalisé “lasagna” dans le modèle JSON du dashboard.

L’édition du JSON se fait dans Dashboard settings → JSON model.

L’objectif est d’ajouter un bloc du type :

text
"lasagna": {
  "fake_uptime": "1234s",
  "note": "this is not real uptime data"
}
Problème rencontré

Malgré plusieurs essais (ajout du champ à la racine du JSON, vérification des virgules, sauvegarde via “Save changes” puis “Save dashboard”), le champ “lasagna” disparaît après sauvegarde.

Après échange avec l’enseignant, ce point n’est pas bloquant pour la validation du TP : le dashboard, les panels et la collecte de métriques fonctionnent correctement, et l’absence effective du champ “lasagna” n’impacte pas le monitoring.

Limitations actuelles

Le champ personnalisé “lasagna” n’est pas présent de manière persistante dans le JSON du dashboard.

Le reste du TP (déploiement Docker, collecte Prometheus, affichage des métriques dans Grafana) fonctionne comme attendu.

Pistes d’amélioration

Comprendre plus précisément pourquoi Grafana rejette les modifications du JSON (dashboard potentiellement provisionné, validation stricte du schéma, etc.).

Ajouter d’autres panels (erreurs HTTP, latence, nombre de requêtes) si de nouvelles métriques sont disponibles.

Mettre en place une sauvegarde automatique des dashboards via export JSON ou provisioning.

