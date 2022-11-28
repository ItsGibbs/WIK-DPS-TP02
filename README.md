# WIK-DPS-TP02
1) J'utilise debian 11.5 et appelle le stage build : 
FROM debian:11.5 as build

2) Je met à jour les packages par défaut d'ubuntu puis installe nodejs et clear le cache : 
RUN apt-get update -yq
&& apt-get install curl gnupg -yq
&& curl -sL https://deb.nodesource.com/setup_16.x | bash
&& apt-get install nodejs -yq
&& apt-get clean -y

3) J'ajoute un dossier add que je défini comme espace de travail : 
ADD . /app/ 
WORKDIR /app

4) J'installe npm et express : 
RUN npm install -g npm && npm install express

5) Pour cet exemple je fini en lanceant le serveur en tant que root, il faudrait le faire avec un autre utilisateur en situation réelle : 
CMD npm run start 
USER root

Pour le multi-stage tout est pareil, puis je créer le stage exec : 
FROM build as exec

Je lance le serveur en tant que root: 
CMD npm run start
USER root

Les commandes à utiliser :

Pour créer l'image avec un seul stage : docker build -t tp2 .
Pour créer l'image avec 2 stages : docker build -t tp2 -f Dockerfile.2 .
Pour créer et lancer un conteneur depuis cette image sur le port d'écoute du serveur : docker run -d -p 3000:3000 tp2
