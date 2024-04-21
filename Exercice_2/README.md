# DOCKERFILE 

Vous devez d'abord créer un Dockerfile personnalisé dans ce répertoire, depuis Drupal:9

Ensuite, exécutez la commande suivante pour installer git : `apt-get update && apt-get install -y git`

N'oubliez pas de nettoyer après votre installation apt avec `rm -rf /var/lib/apt/lists/*` et utilisez `\` et `&&` correctement. 

Puis changez de répertoire de travail pour se placer dans `/var/www/html/themes`

Utilisez ensuite git pour cloner le thème avec la commande `git clone --branch 8.x-4.x --single-branch --degree 1 https://git.drupalcode.org/project/bootstrap.git`

Combinez cette commande avec la commande suivante : `chown -R www-data:www-data bootstrap`. 
  
Ensuite, juste pour être sûr, remettez le répertoire de travail à sa valeur par défaut (à partir de l'image Drupal) dans /var/www/html

# DOCKER-COMPOSE 

Créez un fichier docker-compose.yml composé de 2 services. 

Le premier est construit à partir de l'image que vous venez de créer. 

Le port que l'on souhaite exposer depuis l'hôte est 8080. Celui du conteneur est 80. 

Assurez-vous de configurer la variable d'environnement POSTGRES_PASSWORD sur le service Postgres

Utilisez des volumes pour stocker les données "importantes" de Drupal (référez-vous à la documentation pour savoir où sont les données pertinentes !).  Pour rappel, on cherche généralement à conserver les données de configuration, pour ne pas avoir à reconfigurer le service à chaque nouveau déploiement, et les données d'application (BDD, fichiers de médias, logs, etc.)

Réalisez la configuration/installation de Drupal pour vous assurer de son bon fonctionnement. 