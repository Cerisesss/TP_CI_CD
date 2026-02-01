# TP DevOps – Wordpress

## Objectif du TP

Ce TP a pour objectif de mettre en place le CI/CD permettant de :  
- Déployer une application WordPress et MySQL avec Docker Compose
- Générer automatiquement des manifestes Kubernetes à l’aide de Kompose
- Déployer automatiquement l’archive sur un serveur

L’ensemble du processus est automatisé à chaque push sur la branche main.

## Services Docker

Ce projet contient deux services:
- **MySQL 8**
  - Stockage via volume Docker
  - Utilisation de **Docker Secrets** pour les mots de passe
  
- **WordPress 6.4**
  - Connecté à MySQL
  - Accessible via http://localhost:8080

## Gestion des secrets

### Docker Secrets

Les mots de passe sont gérés via Docker Secrets :
- mysql_root_password
- mysql_wp_password

Ces secrets doivent être créés en externe avant le déploiement.

### GitHub Secrets

Le déploiement final envoie l’archive ZIP vers le dossier **/RenduDevopsKube/** du serveur FTP configuré via les secrets GitHub :
- FTP_HOST
- FTP_USER
- FTP_PASSWORD


## Pipeline CI/CD (GitHub Actions)

### Étapes du pipeline

- Validation Docker Compose
- Génération des manifestes Kubernetes avec l'installation de Kompose et conversion du docker-compose.yml en manifestes Kubernetes. Sauvegarde ensuite les manifestes dans le dossier k8s/
- Packaging : compression des manifestes Kubernetes en archive ZIP
- Déploiement FTP : envoi de l’archive ZIP vers un serveur FTP en utilisation les secrets GitHub pour les identifiants FTP

