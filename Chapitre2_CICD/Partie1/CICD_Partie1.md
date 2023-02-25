# Introduction

Dans les parties précédentes nous avons découvert comment, via l'utilisation des différents outils Docker, conteneuriser notre solution Vault.  


Mais comment pouvons-nous mettre à jour cette image automatiquement afin qu'elle puisse suivre les mises à jour effectuées dans le code ? Par exemple suite à la complétion d'une Pull-Request vers la branche principale de notre repository Github.  

Dans cette avant-dernière partie, nous allons utiliser les outils mis à disposition par Github afin d'implémenter une CI/CD permettant d'effectuer automatiquement ces actions.  
L'objectif sera d'implémenter un worklow fonctionnel d'intégration et de livraison continue grâce aux "Github Actions".  
Ce workflow se limitera à la livraison d'une nouvelle version de la solution et ne comprendra pas la partie déploiement sur un serveur physique ou distant.  


Vous allez avoir besoin d'effectuer un fork de notre [repository Github](https://github.com/a-chatelard/FYC-dock-co) afin de pouvoir mettre en place votre propre workflow et de vous exercer durant les différentes activités présentes dans ce chapitre.  

![intro CICD](../../images/CICD/intro/Capture%20d’écran%202022-12-26%20154616.png)

# Rappels - CI/CD

## L'approche CI/CD
*CI / CD pour "Continuous Integration" (intégration continue) et "Continuous Delivery" (livraison continue) ou plus rarement "Continuous Deployement" (déploiement continue)*

L'approche CI/CD est une approche favorisant la mise en place d'une automatisation lors des étapes de build, de test et de livraison/déploiement d'une solution.  


Cette approche offre plusieurs avantages :

- Une détection d'anomalies et de bugs améliorée et précoce
- Une productivité améliorée
- Des cycles de release plus court  

La CI/CD s'appuie majoritairement sur un mécanisme de "pipeline" représentant une suite d'actions effectuées permettant cette intégration et cette livraison :  

![CICD](../../images/CICD/intro/CICD_CICD.png)  

Dans un fonctionnement classique, chacune de ces parties est gérée par une équipe différente. Cette organisation prend plus de temps car il est nécessaire de synchroniser chacun des acteurs de la chaîne, les coordonner, planifier les actions à effectuer, etc... .  

L'objectif de l'approche CI/CD est donc de réduire au maximum les délais de chacune de ces étapes en les automatisant.  


## L'intégration Continue (CI)
Durant le développement d'une solution informatique, un ou plusieurs développeurs implémentent de nouvelles fonctionnalités en parallèle.  
Ces implémentations sont régulièrement poussées sur une branche principale d'un SCM (tel que Git) hébergé sur un serveur d'intégration (tel que Github Actions, Jenkins, Gitlab, Azure Devops, etc...).
Lorsque le serveur d'intégration détecte cette action, il va automatiquement lancé des étapes de Build et de Test de la nouvelle version du code.  
On vérifie donc le bon fonctionnement de la solution le plus fréquemment possible afin d'empêcher la livraison d'une version buggée de cette dernière.  

![CICD](../../images/CICD/intro/continuous-integration-2%20(1).png)  

Si les étapes de build et de test se terminent avec succès, notre serveur d'intégration enclenchera ensuite une étape de création d'une nouvelle Release.  

## Livraison Continue (CD)
Les étapes de livraison continue arrive après celles de l'intégration continue.  
Nous avons validé que la nouvelle version du logiciel ne provoque pas de bugs ou de problèmes lors du build et une nouvelle Release est prête à être créée.  

Notre Release représentera donc une nouvelle version du logiciel empaquetée que l'on pourra pour déployer si nécessaire sur des environnements de développement, de recette ou de production.
Par exemple, la release d'une nouvelle version d'une application mobile représente un nouveau fichier .apk qui pourra être déployée sur le Play Store ou l'Apple Store.  

Il existe de nombreuses manières de mettre à disposition cette nouvelle version. Dans ce chapitre par exemple, nous la mettrons à disposition sur le Docker Hub via l'utilisation d'images.  


# Rappels - Github Actions

L'outil qui sera utilisé dans les exercices suivant afin de mettre en place notre propre CI/CD est Github Actions.

Github Actions est un outil compris dans Github, permettant de créer des workflows.  

## Workflow
Un workflow représente un processus automatique et configurable exécutant un ou plusieurs jobs ou "travaux".  
Un workflow est défini dans un fichier Yaml présent directement dans le repository Github dans un répertoire dédié.  

Ce dernier se déclenche lorsqu'un évènement ("event") au sein du repository se produit mais peut-être également déclenché périodiquement et manuellement.  

## Event
Un évènement représente une action spécifique ayant eu lieu au sein du repository Github.  
Par exemple, lorsqu'un collaborateur du repository Github crée un Pull-Request, ouvre un ticket ("Issue") ou pousse un commit dans le dépôt distant.  

## Job
Un Job est un ensemble défini d'étapes ("steps") qui peuvent être exécutées dans un environnement particulier. Les jobs peuvent être exécutés en parallèle ou de manière séquentielle, et ils peuvent être contrôlés par divers types de triggers.  
Chaque job possède un nom unique, et il peut être constitué d'une série de steps, qui sont des actions individuelles qui sont effectuées dans le cadre du travail.  

Voici plusieurs exemples de jobs définissables au sein d'un workflow :  

- Build et Test. Le résultat de cette étape défini si la nouvelle version du code peut être déployée ou non
- Livraison et/ou Déploiement.
- Envoi de notification (par mail ou autres canaux). Par exemple lors de la fin de l'exécution du workflow.
- Vérification de la sécurité. Il est possible d'exécuter des scans vérifiant que le code est sécurisé.
- Nettoyage des ressources, en supprimant les fichiers temporaires ou les anciens builds qui ne sont plus utilisés.






