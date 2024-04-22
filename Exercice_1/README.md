# Instructions

Ce répertoire contient une application Node.js et vous devez la faire fonctionner dans un conteneur.

Aucune modification de l'application n'est nécessaire, uniquement des modifications du Dockerfile dans ce répertoire.

## Exercice !

- Créez un compte Gitlab.com.
- Créez un nouveau projet, et avec Git, poussez l'application.
- Créez un fichier .gitlab-ci.yml depuis l'inteface web de Gitlab (Build>Pipeline editor) et choisissez un template Docker. Regardez le contenu du template.
- Imaginez qu'un autre développeur vous ait donné les instructions ci-dessous pour créer le conteneur : construisez le dockerfile associé, faites des commits, et observez le comportement de la pipeline de CICD.
- Télécharger l'image créée sur le Container Registry Gitlab, et vérifiez que l'image se lance bien.

## Instructions du développeur de l'application

- vous devez utiliser l'image officielle `node`, avec la branche alpine 6.x (`node:6-alpine`)
   - (Oui, il s'agit d'une vieille image, mais toutes les images officielles sont toujours disponibles pour toujours sur Docker Hub, pour garantir que même les anciennes applications fonctionnent toujours. Il est courant de devoir encore déployer d'anciennes versions d'application, même des années plus tard. )
- Cette application écoute sur le port 3000, mais le conteneur doit écouter sur le port 80 de l'hôte Docker, il répondra donc à [http://localhost:80](http://localhost:80) sur votre ordinateur
- Ensuite, il doit utiliser le gestionnaire de paquets alpine pour installer tini : `apk add --no-cache tini`.
- Ensuite, il doit créer le répertoire /usr/src/app pour les fichiers d'application avec `mkdir -p /usr/src/app`, ou avec la commande Dockerfile `WORKDIR /usr/src/app`.
- Node.js utilise un "gestionnaire de packages", copiez le fichier package.json dans le conteneur.
- Ensuite, il doit exécuter 'npm install' pour installer les dépendances de ce fichier.
- Pour le garder propre et petit, exécutez `npm cache clean --force` après ce qui précède, dans la même commande RUN.
- Ensuite, il doit copier tous les fichiers du répertoire actuel dans l'image.
- Ensuite, il doit démarrer le conteneur avec la commande `/sbin/tini -- node ./bin/www`. Assurez-vous d'utiliser la syntaxe de tableau JSON pour CMD. (`CMD [ "quelque chose", "quelque chose" ]`)
