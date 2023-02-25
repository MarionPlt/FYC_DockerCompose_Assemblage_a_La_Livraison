# Scalabilité

Nous vous avons montré lors de ce cours comment l'application Vault peut être lancée de façon automatisée grâce à Docker compose.  

Nous pourrions aller plus loin en nous posant les questions suivantes :
- si j'ai un fort trafic sur mon application, est ce que je peux monter plusieurs conteneurs?
- si un conteneur tombe, est ce que je peux en remonter un autre automatiquement?
- si le trafic sur mon application diminue, est ce que je peux supprimer des conteneurs?
- si je veux mettre à jour mon image, est ce que je peux éviter de redémarrer tous mes conteneurs d'un coup?


Les réponses sont amenées par une étude plus poussée de l'orchestration de conteneurs avec des outils tels que Kubernetes, Docker Swarm, Rancher ou OpenShift par exemple !   