TP Docker,docker-compose,k8s
============================

> Rendu attendu : les 3 Dockerfile, le fichier docker-compose.yml et les fichiers et/ou commandes de configuration k8s décris ci-dessous.

Le but de ce TP est de déployer l'application contenue dans ce répository dans une stack de conteneurs Docker.
L'application est composée de 3 composants : `vote`, `result` et `worker`.

- `vote` est un composant hébergeant une UI permettant de voter
- `result` est un composant hébergeant une UI affichant les résultats de vote
- `worker` est un composant gérant le backend des votes

L'application totale sera déployée dans une stack composée de 5 services :

- un service `vote` buildé depuis `./vote`----
  + utilisant les réseaux `front-tier` et `back-tier`-----
  + exposant le port  `80` sur le port `5000` de l’hôte ------
  + monte le répertoire `./vote` comme volume du conteneur dans `/app` ---
- un service `result` buildé depuis `./result`----
  + utilisant les réseaux `front-tier` et `back-tier`---
  + exposant le port  `80` sur le port `5001` de l’hôte---
  + exposant le port  `5858` sur le port `5858` de l’hôte---
  + monte le répertoire `./result` comme volume du conteneur dans `/app`----
- un service `worker` buildé depuis `./worker`----
  + utilisant le réseau `back-tier`----
  + dépendant des 2 derniers services----
- un message broker nommé `redis` utilisant l’image `redis:alpine` pour gérer les interactions entre les composants---
  + utilisant le réseau `back-tier`---
  + exposant le port  `6379`---
- une base de données nommée `db` utilisant l’image `postgres:9.4` pour persister les votes----
  + utilisant le réseau `back-tier`----
  + un volume externe persistant le chemin `/var/lib/postgresql/data`---
  + Les variables d’environment:---
    - `POSTGRES_USER: "postgres"`---
    - `POSTGRES_PASSWORD: "postgres"`---

Pour permettre ce déploiement, il faudra donc :

- un fichier Dockerfile pour chacun des 3 composants `vote`, `result` et `worker`. Un template est fourni pour chacun de ces services dans le répertoire de chaque composant.
- créer un fichier de configuration docker-compose.yml décrivant le déploiement de la stack
- dans un second temps, déployer la même stack applicative dans k8s. Pour améliorer les performances, on déploiera 2 conteneurs `vote`. On privilégiera l'utilisation de fichiers de configuration Yaml ou Json et on utilisera `kubectl` uniquement pour appliquer ces fichiers (et non pour configurer directement les pods).

Une fois déployée, l'application expose :

- une UI pour les votes à l'adresse http://localhost:5000
- une UI pour afficher les résultats à l'adresse http://localhost:5001
