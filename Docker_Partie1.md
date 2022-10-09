# Partie 1: premiers pas avec Docker   

## Pourquoi Docker?  
**"Mais pourtant ça marche chez moi!"**  
On a tous rencontré ce problème en travaillant sur un projet à plusieurs. Le fameux projet qui fonctionne bien chez une personne et pas chez une autre.
Le projet qui fonctionne en mode "développement" et qui ne fonctionne plus en mode "production".
Et si on livrait plus que du code? Et si on livrait tout un package pour lancer notre application facilement? 
   

## Présentation de Docker Desktop  
Docker est un outil qui permet de lancer un processus sur votre machine de façon isolée, dans un *conteneur*.    
Il peut être créé, démarré, arrêté, déplacé d'un environnement à un autre ou supprimé grâce à la CLI.  
Un conteneur peut être lancé sur une machine locale mais aussi sur une machine virtuelle ou déployé dans le cloud, ce qui offre une grande flexibilité.    
Chaque conteneur est isolé des potentiels autres conteneurs et fait tourner ses propres logiciels, binaires et ses propres configurations.  

Un conteneur est une instance executable d'une *image*.
Une image Docker fournit le système de fichier dédié pour le conteneur.  
L'image doit donc avoir tout ce dont le conteneur aura besoin pour fonctionner : 
* dépendances 
* configuration
* scripts 
* binaires
* variables d'environnement
* commandes par défaut à executer
* ...  
Cette image peut être stockée localement ou sur le DockerHub.  
Enfin, Docker peut tourner sur n'importe quelle OS.

  
**-----!!INSERT SCHEMA HERE!!-----**

## Commandes de base
Voyons ici quelques commandes basiques que nous allons réutiliser tout le long du cours.  
Cette liste est loin d'être exhaustive, n'hésitez pas à consulter la [documentation Docker](https://docs.docker.com/) pour découvrir toutes les commandes qui sont à votre disposition!

Nous allons tester plusieurs de ces commandes avec l'image *helloworld* de Docker. 
  
### docker image 
`docker image build`  
`docker image ls`  
`docker image rm`  
`docker image prune`  

### docker container
`docker container ls` ou `docker ps` ou `docker ps -a`  
`docker container exec`  
`docker container kill`  
`docker container logs`  
`docker container prune`  
`docker container restart`  
`docker container rm`  
`docker container run`  
`docker container start`  
`docker container stop`  

### docker run
`docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`  

`--detach`, `-d`  
`--name`  
`--publish`, `-p`  
`--user`, `-u`  