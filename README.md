# GLPI avec Docker

Déploiement de [GLPI](https://glpi-project.org/) (gestion de parc informatique et helpdesk) avec Docker Compose et MariaDB.

## Prérequis

- [Docker](https://docs.docker.com/get-docker/)
- Docker Compose (inclus dans Docker Desktop)

## Installation

1. Cloner le dépôt :
   ```bash
   git clone https://github.com/fmassol-63/glpi-docker.git
   cd glpi-docker
   ```

2. Créer le fichier de configuration :
   ```bash
   cp .env.example .env
   ```

3. Modifier le fichier `.env` et remplacer les `changeme` par des mots de passe solides :
   ```env
   MYSQL_ROOT_PASSWORD=changeme
   MYSQL_DATABASE=glpi
   MYSQL_USER=glpi_user
   MYSQL_PASSWORD=changeme
   ```

4. Lancer les conteneurs :
   ```bash
   docker compose up -d
   ```

5. Ouvrir GLPI dans le navigateur : http://localhost:8080

## Configuration de la base de données

Dans l'assistant d'installation de GLPI, renseigner :

| Champ | Valeur |
|---|---|
| Serveur SQL | `mysql` |
| Utilisateur | valeur de `MYSQL_USER` |
| Mot de passe | valeur de `MYSQL_PASSWORD` |
| Base de données | valeur de `MYSQL_DATABASE` |

## Comptes par défaut

| Utilisateur | Mot de passe |
|---|---|
| glpi | glpi |
| tech | tech |
| normal | normal |
| post-only | postonly |

**Important : changer ces mots de passe dès la première connexion.**

## Commandes utiles

```bash
docker compose up -d       # Démarrer
docker compose down        # Arrêter
docker compose logs -f     # Voir les logs
docker compose pull        # Mettre à jour les images
```

Pour tout supprimer, y compris les données :
```bash
docker compose down -v
```

## Sécurité

- Le fichier `.env` contient les mots de passe : il ne doit **jamais** être publié sur GitHub (il est dans le `.gitignore`).
- Ne pas exposer GLPI sur Internet sans HTTPS (reverse proxy comme Nginx ou Traefik).
- Faire des sauvegardes régulières de la base de données.

## Sauvegarde de la base

```bash
docker exec glpi-db mariadb-dump -u root -p glpi > sauvegarde.sql
```

## Licence

Ce projet est un simple déploiement Docker. GLPI est distribué sous licence GPL v3.
