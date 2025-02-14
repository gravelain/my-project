# 📘 My Project - The tip top 

Ce projet est une plateforme web de jeu concours permettant aux utilisateurs de remporter des lots grâce à un code de jeu obtenu sur leur ticket de caisse. Il utilise Docker Compose v2 pour orchestrer plusieurs services, incluant une base de données PostgreSQL, un backend NestJS, un frontend Next.js, ainsi que des outils de monitoring et d'intégration continue comme Prometheus, Grafana, SonarQube, Jenkins et Traefik.
Le déploiement est fait sur notre vps : AWS

---

## 📂 Structure du Projet

```bash
my-project/
├── backend/       # Code source du backend (NestJS)
├── frontend/      # Code source du frontend (Next.js)
├── grafana/       # Configuration Grafana
├── jenkins/       # Configuration Jenkins
├── monitoring/    # Fichiers de configuration pour Prometheus
├── prometheus/    # Configuration Prometheus
├── scripts/       # Scripts utilitaires
├── sonarqube/     # Configuration SonarQube
├── traefik/       # Configuration Traefik
├── docker-compose.yml             # Configuration principale des services Docker
├── docker-compose.dev.yml         # Configuration spécifique pour l'environnement de développement
├── docker-compose.preprod.yml     # Configuration pré-production
├── docker-compose.prod.yml        # Configuration production
└── .env.*                         # Fichiers d'environnement
```

---

## 🛠 Technologies Utilisées

- **NestJS** : Framework Node.js utilisé pour construire des API backend performantes et évolutives.
- **Next.js** : Framework React pour le frontend, offrant un rendu côté serveur (SSR) et des performances optimisées.
- **PostgreSQL** : Base de données relationnelle robuste et performante.
- **Docker & Docker Compose** : Conteneurisation et orchestration des services.
- **Jenkins** : Serveur CI/CD permettant l’automatisation des déploiements et des tests.
- **Prometheus & Grafana** : Outils de monitoring et visualisation des métriques.
- **SonarQube** : Analyse statique du code pour améliorer la qualité et détecter les vulnérabilités.
- **Traefik** : Reverse proxy et gestionnaire de certificats pour l'exposition sécurisée des services.

---

## 🛠 Services Définis

### Base de Données (PostgreSQL)
- Image : `postgres:15-alpine`
- Port : `5432`
- Stockage persistant : `db-data:/var/lib/postgresql/data`

### Backend (NestJS)
- Construit depuis `./backend`
- Variables d'environnement :
  - `NODE_ENV`
  - `DATABASE_URL=postgres://admin:admin@db:5432/mydatabase`
  - `JWT_SECRET=mysecretkey`
- Port : `4000`

### Frontend (Next.js)
- Construit depuis `./frontend`
- Port : `3000`

### Monitoring (Prometheus & Grafana)
- **Prometheus** : `prom/prometheus:v2.45.0`, exposé sur `9090`
- **Grafana** : `grafana/grafana-oss:10.2.2`, exposé sur `3001`

### Analyse de Code (SonarQube)
- Image : `sonarqube:lts`
- Port : `9000`
- Base de données dédiée sur PostgreSQL

### Intégration Continue (Jenkins)
- Image : `jenkins/jenkins:lts-jdk17`
- Ports : `8080`, `50000`

### Proxy Inverse (Traefik)
- Image : `traefik:v2.5`
- Port : `80`
- Configuration : Activation du mode API et détection automatique des conteneurs Docker

---

## 🔄 CI/CD & Déploiement
L'intégration continue et le déploiement sont gérés avec **Jenkins** sur un **VPS**. Jenkins est configuré pour :
- Exécuter des tests automatisés (linting, unit tests, intégration...)
- Construire et pousser des images Docker
- Déployer l'application sur le serveur VPS

Grâce à cette infrastructure, les nouvelles versions du projet sont automatiquement testées et déployées en production de manière sécurisée et optimisée.

---

## 🚀 Utilisation avec Docker Compose

### 🌟 Définition de l'environnement
Ajoutez cette fonction à votre `.bashrc` ou `.zshrc` :

```bash
dcenv() {
  case "$1" in
    dev|preprod|prod)
      export ENV=$1
      export ENV_FILE=".env.$ENV"
      echo "✅ Environnement défini sur : $ENV"
      ;;
    *)
      echo "❌ Spécifie un environnement valide : dev, preprod ou prod"
      return 1
      ;;
  esac
}
```


### 🌟 Choisir son environnement
En fonction de si vous souhaitez travailler en dev, preprod ou prod, il faudrait selectionner son 
environnement de travail avant de démarrer les services : choisi ton env et tape la commande appropriée! 
```bash
dcenv dev  #  ✅ Environnement défini sur : dev
```

```bash
dcenv preprod  #  ✅ Environnement défini sur : preprod
```


```bash
dcenv prod  #  ✅ Environnement défini sur : prod
```


#### 🏗 Démarrer les services
```bash
dcu  # Up (démarrage des services en arrière-plan)
```

#### ❌ Arrêter les services
```bash
dcd  # Down (arrêt des services)
```

#### 🔄 Redémarrer les services
```bash
dcr  # Restart
```

#### 🔍 Voir les logs
```bash
dcl  # Logs globaux
```

#### 🎯 Démarrer un service spécifique
```bash
dbackend  # Backend
dfrontend # Frontend
ddb       # Base de données
```

#### 📜 Logs spécifiques
```bash
dlogs_backend  # Backend
dlogs_frontend # Frontend
dlogs_db       # Base de données
```

#### 🖥 Accéder aux conteneurs
```bash
dbash  # Ouvrir un shell bash dans le backend
dsh    # Ouvrir un shell sh dans le backend
dpsql  # Ouvrir PostgreSQL avec psql
```

---

## 📌 Remarque
Assurez-vous d'avoir Docker et Docker Compose v2 installés sur votre machine avant d'exécuter ces commandes. Vous pouvez vérifier les versions avec :

```bash
docker --version
docker compose version
```

Let's Go ! 🚀

