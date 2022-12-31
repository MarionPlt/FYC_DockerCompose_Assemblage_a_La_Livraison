# CI/CD Partie 4 : nouvelle image DockerHub

Petit retour en arrière : dans la partie DockerHub, nous avons mis à jour le docker-compose.yaml en remplaçant les images
locales par des images distantes. Grâce à ça, quand un utilisateur veut lancer l'application N-tier, il n'a plus qu'une
seule commande à exécuter (``docker compose up``), et il n'a plus à forger des images locales à la main.

Maintenant que nous avons mis à jour notre code et que nous avons fait une release sur Github, il faut mettre à jour
les images distantes sur Docker Hub. Ainsi, les updates réalisés sur l'application ne demandent pas de commande
supplémentaire à l'utilisateur. Il fera toujours uniquement un ``docker compose up``, qui ira chercher l'image qui porte le tag ``latest``.

## Générer un token d'identification sur Docker Hub

Nous allons avoir besoin d'un token d'identification pour la suite de notre workflow sur Github.
Allez dans les paramètres utilisateur et allez dans Account Settings