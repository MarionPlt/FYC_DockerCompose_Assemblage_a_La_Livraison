# Partie 1: premiers pas avec Docker   

## Présentation de Docker Desktop  
Docker est un outil qui permet de lancer un processus sur votre machine de façon isolée, dans un *conteneur*.
On peut faire tourner Docker sur n'importe quelle OS.   
Un conteneur est une instance executable d'une *image*. Ce conteneur peut être créé, démarré, arrêté, déplacé ou supprimé grâce à la CLI.  
Un conteneur peut être lancé sur une machine locale mais aussi sur une machine virtuelle ou déployé dans le cloud.  
Un conteneur est isolé des potentiels autres conteneurs et fait tourner ses propres logiciels, binaires et ses propres configurations.  

Une image Docker fournit le système de fichier dédié pour le conteneur.  
L'image doit donc avoir tout ce dont le conteneur aura besoin pour fonctionner : 
* dépendances 
* configuration
* scripts 
* binaires
* variables d'environnement
* commandes par défaut à executer
* ...

**-----!!INSERT SCHEMA HERE!!-----**

## Commandes de base
Voyons ici quelques commandes basiques que nous allons réutiliser tout le long du cours.  
Cette liste est loin d'être exhaustive, n'hésitez pas à consulter la [documentation Docker](https://docs.docker.com/) pour découvrir toutes les commandes qui sont à votre disposition!
  
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