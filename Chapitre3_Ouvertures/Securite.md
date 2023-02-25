# Sécurité dans Docker
Pendant la première partie de ce cours, nous vous avons présenté l'outil Docker pour en maitriser les commandes principales. 

Nous allons maintenant parler de sécurité dans Docker.  


## Rappel sur les images Docker
Lorsque vous cherchez une image sur Docker Hub, prenez le temps de vérifier sa documentation :

- l'image est maintenue régulièrement
- si possible c'est une image officielle
- l'image est documentée

Ces étapes sont importantes pour éviter de lancer des conteneurs contenant des failles de sécurité !


## Docker Rootless
Par défaut votre daemon Docker et tous les conteneurs sont lancés en mode root.  

Pour limiter les potentielles vulnérabilités de votre daemon ou de l'un de vos conteneurs, vous pouvez utiliser le mode rootless.  

Ce mode vous permettra de lancer le daemon et les conteneurs dans un user namespace afin de ne pas avoir accès à des privilèges trop élevés.   