# Todo-App---D-mo-Microservices-avec-Docker


Application de gestion de tâches en architecture microservices, parfaite pour une démonstration Docker et Docker Compose.

Architecture
┌─────────────┐
│   Frontend  │ (React + Vite)
│   Port 80   │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ API Gateway │ (Node.js/Express)
│  Port 3000  │
└──────┬──────┘
       │
       ├─────────────────┐
       │                 │
       ▼                 ▼
┌─────────────┐   ┌─────────────┐
│User Service │   │Task Service │
│  Port 3001  │   │  Port 3002  │
└──────┬──────┘   └──────┬──────┘
       │                 │
       └────────┬────────┘
                ▼
        ┌─────────────┐
        │   MongoDB   │
        │  Port 27017 │
        └─────────────┘

## Structure du projet

microservices-todo-app/
├── docker-compose.yml
├── api-gateway/
│   ├── Dockerfile
│   ├── package.json
│   └── index.js
├── user-service/
│   ├── Dockerfile
│   ├── package.json
│   └── index.js
├── task-service/
│   ├── Dockerfile
│   ├── package.json
│   └── index.js
└── frontend/
    ├── Dockerfile
    ├── nginx.conf
    ├── package.json
    ├── vite.config.js
    ├── index.html
    └── src/
        ├── main.jsx
        ├── App.jsx
        ├── App.css
        └── index.css

## Démarrage rapide
Prérequis

Docker Desktop installé
Docker Compose installé

Lancer l'application

Cloner ou créer la structure du projet avec tous les fichiers fournis
Construire et lancer tous les services :

bashdocker-compose up --build

Accéder à l'application :


Frontend : http://localhost
API Gateway : http://localhost:3000
User Service : http://localhost:3001
Task Service : http://localhost:3002
MongoDB : localhost:27017

## Commandes Docker utiles
Gestion des conteneurs
bash# Lancer en arrière-plan
docker-compose up -d

# Arrêter les services
docker-compose down

# Voir les logs
docker-compose logs -f

# Voir les logs d'un service spécifique
docker-compose logs -f frontend

# Redémarrer un service
docker-compose restart user-service

# Voir les conteneurs en cours
docker-compose ps
Rebuild et nettoyage
bash# Rebuild sans cache
docker-compose build --no-cache

# Supprimer tout (conteneurs, réseaux, volumes)
docker-compose down -v

# Nettoyer les images non utilisées
docker system prune -a
🔍 Tests des API
User Service
bash# Créer un utilisateur
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Jean Dupont","email":"jean@example.com"}'

# Lister les utilisateurs
curl http://localhost:3000/api/users

# Supprimer un utilisateur
curl -X DELETE http://localhost:3000/api/users/USER_ID
Task Service
bash# Créer une tâche
curl -X POST http://localhost:3000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title":"Ma tâche","description":"Description","userId":"USER_ID"}'

# Lister les tâches d'un utilisateur
curl http://localhost:3000/api/tasks?userId=USER_ID

# Marquer une tâche comme complétée
curl -X PUT http://localhost:3000/api/tasks/TASK_ID \
  -H "Content-Type: application/json" \
  -d '{"completed":true}'

  
## Points de démonstration Docker
1. Isolation des services
Chaque microservice tourne dans son propre conteneur, isolé des autres.
2. Communication inter-conteneurs
Les services communiquent via le réseau Docker todo-network.
3. Gestion des dépendances
Utilisation de depends_on pour gérer l'ordre de démarrage.
4. Variables d'environnement
Configuration flexible via les variables d'environnement.
5. Volumes persistants
Les données MongoDB sont persistées avec des volumes Docker.
6. Multi-stage builds
Le frontend utilise un build multi-étapes (Node → Nginx).
7. Scaling horizontal
bash# Lancer 3 instances du task-service
docker-compose up -d --scale task-service=3

# Debugging
Accéder à un conteneur
bashdocker exec -it user-service sh
docker exec -it mongodb mongosh
Voir les logs en temps réel
bashdocker-compose logs -f --tail=100
Inspecter le réseau
bashdocker network inspect microservices-todo-app_todo-network

##Fonctionnalités de l'interface

✅ Créer et gérer des utilisateurs
✅ Créer des tâches pour chaque utilisateur
✅ Marquer les tâches comme complétées
✅ Supprimer des utilisateurs et des tâches
✅ Interface responsive et moderne
✅ Affichage de l'architecture en footer

# Pour votre présentation
Points clés à mentionner :

Découplage : Chaque service a sa propre responsabilité
Scalabilité : Possibilité de scaler chaque service indépendamment
Résilience : Si un service tombe, les autres continuent
Développement : Équipes peuvent travailler indépendamment
Déploiement : Mise à jour d'un service sans affecter les autres

Démonstration suggérée :

Montrer le docker-compose.yml
Lancer docker-compose up
Montrer l'interface web
Faire docker-compose ps pour voir les conteneurs
Montrer les logs avec docker-compose logs
Arrêter un service : docker-compose stop task-service
Montrer que l'app continue à fonctionner partiellement
Redémarrer : docker-compose start task-service

## Credentials MongoDB

Username: admin
Password: password123
Database User Service: users
Database Task Service: tasks


Note : Cette application est conçue à des fins de démonstration. Pour la production, ajoutez l'authentification, la sécurité, et une gestion d'erreurs plus robuste.
