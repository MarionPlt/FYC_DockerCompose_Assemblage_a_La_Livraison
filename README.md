# FindYourCourse_Documentation

Project ESGI M2 Find Your Course  : "DOCKER COMPOSE : DE L'ASSEMBLAGE D'IMAGES A LA LIVRAISON"

# Prérequis
Pour appréhender ce cours vous devez savoir utiliser Git et avoir des notions de **CI/CD**.

De plus, Git doit être installé sur votre machine. Un IDE n'est pas obligatoire si vous savez faire sans, mais c'est fortement recommandé.


# Pourquoi Docker ? 

![Bah ça marche](/images/bahcamarche.jpg)


Il est très fréquent pendant nos études et dans notre entreprise de travailler en groupe. Mais pour partager efficacement notre code, il faut idéalement avoir le même environnement de travail.

Mais dans la réalité, on a plutôt un/e développeur/se qui pousse son code, et les autres qui se battent ensuite pour le lancer sur leur environnement. 



Pour éviter cela, vous pouvez encapsuler votre application, et son environnement précis, dans un conteneur Docker.



Dans une première partie, nous vous proposons d'apprendre comment installer Docker sur votre machine, et des commandes de base pour prendre en main l'outil.

A l'aide d'une application  N-tiers basique (Vault),  vous allez construire les images Docker de chaque tiers applicatif. Pas à pas, vous allez appréhender de nouveaux outils pour automatiser votre création et votre lancement de l'application Vault grâce aux **Dockerfile** et à **Docker compose**. Puis vous allez envoyer ces images sur un répertoire distant (**DockerHub**) et utiliser ces images distantes. 

![Docker compose](/images/compose.jpg)



# Pourquoi Github?
Une fois que vous avez pu mettre de côté les problèmes d'environnement de votre application (merci Docker!) , il s'agit de pouvoir continuer à mettre à jour votre code sans avoir à recréer les images distantes à la main.

Dans une seconde partie, vous allez mettre en place un processus d'intégration continue et de livraison continue (**CI/CD**) grâce à **Github** et aux **Github Actions**.

Etape par étape, vous aller forger le CI/CD de l'application Vault afin de réaliser le build, les tests, la release sur Github et la création de l'image distante Docker de façon automatique. 



# Comment se découpe le cours ? 


![Parcours](/images/parcours.jpg)

Chaque étape contient : 
- une partie théorique
- un exercice accompagné
- un exercice à réaliser seul
- un ou plusieurs QCM


Ne restez pas bloqué ! Tout au long des exercices, vous pourrez vous référer à notre [dépôt Github](https://github.com/a-chatelard/FYC-dock-co) pour la correction : à chaque exercice sa branche de correction ! 

Une courte vidéo d'introduction [ici](https://www.youtube.com/watch?v=BLCrtRmNaNk)

### Vous êtes prêt/e ? C'est parti ! 

