# Partie 2 - Dockerfile

## Introduction aux Dockerfile

&nbsp;

Docker permet de construire une image Docker d’une application à l’aide de la commande build et d’un Dockerfile. L’image ainsi produite peut être utilisée pour créer un conteneur dans un docker host, ce conteneur étant chargé d’exécuter l’application (ou service) contenue dans l’image.

&nbsp;

*Qu’est-ce qu’un Dockerfile ?*

Un dockerfile est un fichier texte qui contient toutes les instructions nécessaires à la construction d’une image Docker.

&nbsp;

*Pourquoi créer une image Docker ?*

Une image Docker de votre application est nécessaire pour créer un conteneur qui exécutera la dite application dans un environnement Docker.

&nbsp;

*Comment créer un Dockerfile ?*

Un Dockerfile peut être créé avec n’importe quel éditeur de texte. Pour ce faire, il suffit de créer un nouveau fichier nommé « Dockerfile » sans ajouter d’extension (il peut être nécessaire de retirer l’extension « .txt »).

&nbsp;

Il est à noter qu’il n’est pas possible de renommer un fichier Dockerfile : tous les fichiers Dockerfile sans exception doivent impérativement se nommer : « Dockerfile ». Ce qui signifie qu’il n’est pas possible d’avoir plus d’un Dockerfile par dossier !

Nous explorerons dans la prochaine section la structure et les instructions d’un Dockerfile.

&nbsp;

&nbsp;

## Le fichier Dockerfile

&nbsp;

Comme expliqué dans la section précédente, un Dockerfile est un fichier texte comportant une série d’instructions. Ces instructions ressemble à s'y méprendre à des "commandes".

Néanmoins, les instructions utilisées dans les Dockerfile sont spécifiques à ces derniers et ne doivent pas être confondus avec les commandes de la CLI (command line interface) de Docker.

&nbsp;

### 1) Syntaxe

&nbsp;

La syntaxe d’un fichier Dockerfile se veut à la fois simple et accessible, elle obéit néanmoins à des règles qui lui sont propre :

- Il ne peut pas y avoir 2 instructions Dockerfile sur une même ligne.

- Chaque instruction s’exécute de façon indépendante.

&nbsp;

La syntaxe des instructions au sein d’un Dockerfile ressemble à ce que l’on peut trouver dans un script de commande classique :

``INSTRUCTION <argument1> <argument2> …``
 
&nbsp;

Un exemple avec l'instruction COPY :

``COPY aspnetapp/*.csproj ./aspnetapp/ ``
 
&nbsp;

Nous reviendrons sur les détails du fonctionnement de cette instruction dans la prochaine section.

&nbsp;

Bien que cela ne soit pas obligatoire, la convention veut que le nom des instructions d’un Dockerfile soit écrit en majuscule.

&nbsp;

Pour écrire un commentaire dans un Dockerfile, il faut commencer une ligne avec le caractère « # ».

&nbsp; 

Exemple :

``# Commentaire dans un dockerfile``
 
Il n’est pas possible de mettre un commentaire et une instruction sur une même ligne.

&nbsp;

Certaines instructions peuvent être écrites selon deux formes différentes, c’est le cas pour CMD, ENTRYPOINT et RUN. Ces deux formes sont les suivantes :

&nbsp;

la forme « shell ». elle permet d’exécuter une instruction avec le shell du container.

Syntaxe : ``Instruction param1 param2 … ``

&nbsp;

la forme « exec ». elle permet d’exécuter une commande directement, sans passer par le shell. Dans cette forme, les éléments de la commande sont parser dans un tableau JSON. C’est la forme privilégié pour les instructions CMD et ENTRYPOINT.

Syntaxe : ``Instruction [« param1 », « param2 », … ]``

&nbsp;

### 2) Premières instructions

&nbsp;

Voici maintenant quelques explications sur les instructions indispensables à la création de toute image docker.

&nbsp;

#### FROM :
L'instruction ``FROM`` permet de spécifier l’image de base du Dockerfile. En réalité, la première instruction d’un Dockerfile est toujours une instruction ``FROM``.

Cette instruction nous permet de construire notre nouvelle image autour d'un environnement conteneurisable, avec éventuellement le runtime nécessaire à l’exécution de notre application.

&nbsp;

Elle prend la forme suivante :

``FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]``

&nbsp;

``<platform>``  permet de spécifier la plateforme visée pour instancier l’image, généralement cette option est omise.

``<image>`` c’est le nom de l’image qui doit service d’image de base pour notre Dockerfile

``<tag>`` permet de ciblé une version particulière de l’image. Si aucun tag n’est spécifié, c’est la version la plus récente qui est récupérée.

``<name>`` est utilisé pour donner un nom à cette image de base. C'est indispensable si vous souhaiter interagir avec cette image à partir d'une autre image de base, ce que nous verrons plus loin dans ce chapitre.

&nbsp;

Voici quelques exemples :

- Si je souhaites conteneuriser une application PHP 7 je peux par exemple écrire l’instruction suivante au début de mon Dockerfile :

	```
	FROM php:7.0-apache
	```

	Je récupère ainsi une image qui nous permettra d’exécuter une application PHP 7 avec serveur apache.

&nbsp; 

- Si je souhaite conteneuriser une application .NET 6, je peux utiliser l'instruction suivante :

	```
	FROM mcr.microsoft.com/dotnet/aspnet:6.0
	```

	J’aurais ainsi dans mon image un environnement conteneurisable permettant d’exécuter une application .NET 6

&nbsp;

#### COPY :
L'instruction ``COPY`` est une instruction très simple et néanmoins indispensable pour conteneuriser une application.

Elle permet de copier dans la nouvelle image tous les fichiers/dossiers nécessaires au fonctionnement de l’application que vous souhaitez conteneuriser. Les éléments ainsi copiés seront ajoutés au système de fichier du conteneur créer à partir de la nouvelle image.

&nbsp;

Elle prend la forme suivante :

```
COPY [--chown=<user>:<group>] <source> ... <destination>
```

&nbsp;

``<source>`` est un emplacement du système de fichier de votre machine contenant les fichiers à copier.

``<destination>`` est un emplacement dans le système de fichier du futur conteneur.

Si un chemin comporte des espaces ou autres caractères blancs, il faut penser à l’entourer de guillemets. xemple :

``"C:\source\mon projet"`` 

&nbsp; 

Il est possible de spécifier un utilisateur ou un groupe qui auraient des droits spécifiques sur les éléments à copier, ces droits s’appliquant dans le système de fichier du futur conteneur.

&nbsp;

Voici un exemple d’utilisation :

- On souhaite copier le contenu du dossier *aspnetapp* dans un dossier *aspnetapp* présent sur le conteneur.

	```
	COPY aspnetapp/* ./aspnetapp/
	``` 

	Ici tous les fichiers contenus dans le dossier « aspnetapp » (qui doit se trouver dans le même dossier que le dockerfile présent sur votre machine), seront copiés dans un nouveau dossier nommé « aspnetapp » et situé à la racine du système de fichier du futur conteneur.

&nbsp;

#### ENTRYPOINT :

Cette instruction permet de changer notre conteneur en un conteneur exécutable. En effet il permet de spécifier la commande qui sera exécuté lors du démarrage du conteneur.

Comme expliqué précédemment, l’instruction ``ENTRYPOINT`` peut prendre deux formes : la forme « shell » ou la forme « exec ». Ici nous utiliserons la forme « exec », car c’est la forme privilégiée pour cette instruction.

&nbsp; 

L’instruction ``ENTRYPOINT`` prend la forme suivante :

``ENTRYPOINT ["executable", "param1", "param2"]``

&nbsp;

Voici un exemple d’utilisation :

```
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

&nbsp;

### 3) Exemple d’un Dockerfile minimaliste

&nbsp;

Les instructions présentées dans cette section permettent de créer une image de notre application en quelques instructions :

```
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS runtime
COPY published/* ./app
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```
 
&nbsp;

Dans cet exemple, une image du runtime ASP.NET 6 (librairie permettant de créer des applications web en C#) est utilisée comme image de base pour notre nouvelle image. Puis le contenu du dossier « published » (présent sur la machine hôte) est copié dans un nouveau dossier nommé « app » directement dans le système de fichier du futur conteneur. Enfin la commande « dotnet aspnetapp.dll » spécifiée à l'aide de l'instruction ``ENTRYPOINT`` permettra d’exécuter notre application au démarrage du conteneur.

&nbsp;

Nous verrons dans la prochaine section le système de couche des images docker, le cache de Docker, et comment optimiser les images.

&nbsp;

&nbsp;

## Optimisation des fichiers Dockerfile

&nbsp;

### 1) Le cache de Docker et le système en couche

&nbsp;

Avant d’aller plus dans l’exploration des instructions disponibles dans les Dockerfile, il convient de prendre un peu de temps pour expliquer une partie du fonctionnement de la construction des images Docker, et notamment le système de cache (build cache).

Docker est réputé pour son efficacité à sa simplicité d’utilisation pour construire des images.

&nbsp;

Comment Docker peut-il être aussi performant pour la création des images ?

&nbsp;

Docker a été conçu avec un système de cache permettant de rendre la construction d’image aussi rapide que possible. Le système de cache permet de créer des étapes intermédiaires, appelées « couche », qui pourrons être réutilisées le cas échéant pour construire de futures images. En réalité, chaque instruction présente dans un Dockerfile va générer une couche qui sera enregistrée dans le cache durant la construction de l'image.

Chaque couche créée pendant "le build" de l'image est immutable : elle ne peut pas être modifiée par les couches suivantes. Chaque couche étant sauvegardée dans le cache, elle possède un identifiant unique. Si Docker a besoin de l’une d’entre elle pour construire une nouvelle image, il peut l’utiliser directement sans se soucier d’un problème de compatibilité. Au final, chaque image constitue un empilement de plusieurs couches.

&nbsp;

Il est également possible de créer des images sans utiliser le cache de Docker, en ajoutant l’option ``--no-cache=true`` avec la commande « docker build »

&nbsp;

### 2) Optimisation des images :

&nbsp;

Comment optimiser la construction des images Docker ?

&nbsp;

Une première piste peut consister à construire un fichier Dockerfile le plus simple qui soit, dans la mesure du possible, tant que celle-ci produit une image fonctionnelle. Aussi il convient de ne pas alourdir l’image inutilement en copiant dans l’image, à l’aide de l’instruction ``COPY``, des fichiers qui ne seront jamais utilisés.

Il faut également prendre en compte la mécanique du cache de Docker qui rentre en jeu lors de la création d’une image : si une instruction exécutée entraine des modifications par rapport à la couche associée (présente dans le cache), cette couche est regénérée.

&nbsp;

<u>Exemple 1 :</u> vous modifiez un fichier dans votre projet « program.cs ». Dans ce cas l’instruction ``COPY`` qui permet de copier le contenu du projet dans l’image est réexécutée, elle ne peut pas s’appuyer sur le cache.

<u>Exemple 2 :</u> vous utilisez l'instruction ``RUN apt-get update && apt-get install`` pour votre future conteneur, et vous modifiez la commande en y ajoutant un nouvel argument « -y ». La commande a changé, le système de cache ne peut pas être utilisé pour cette couche.

&nbsp;

La modification d’une couche entraine le régénération de toutes les couches suivantes !

&nbsp;

Pour cette raison, on mettra d’abord en début de fichier les instructions qui vont configurer l’environnement du conteneur, notamment avec l’utilisation de l'instruction ``RUN`` ou l'instruction ``ENV`` (ces deux instructions seront présentées dans la section suivante). En effet ces instructions étant moins susceptibles d’être modifiées d’une construction d’image à une autre, les couches existantes dans le cache pourront être réutilisées.

De la même manière, on préféra mettre à la fin du fichier les instructions qui copient des fichiers dans l’image, afin de limiter le nombre de couche qu’il faudra regénérer à la prochaine construction d’image.

&nbsp; 

Attention, certaines couches peuvent sembler ne pas avoir besoin d’être régénérées, notamment les instructions liées à la mise à jour et la récupération de paquets. Néanmoins cela peut constituer un piège dans la conception d’image Docker.

L'instruction présentée précédemment : ``RUN apt-get update && apt-get install`` va créer une image avec l’installation des paquets et leurs mise à jour comme résultat.

Néanmoins dans les constructions d’image futur, comme l’instruction associée n’a pas généré de modification par rapport à la couche existante dans le cache, celle-ci est réutilisée. Par conséquent, l’installation des paquets et des mise à jours n’est pas actualisée, elle correspond aux opérations effectuées lors de la première génération de l’image, lorsque la couche associée à l’instruction RUN a été mise en cache !

&nbsp;

Si l’on souhaite néanmoins regénérer cette couche à chaque construction d’image, pour s’assurer que les paquets soient bien installés dans leurs dernières versions, il est possible de contourner le problème de 3 façons :

- Placer l’instruction ``RUN`` de façon à ce que la couche associée soit systématiquement regénérée (très probable si elle est placée après une instruction ``COPY``).

- Utiliser l’option ``–no-cache`` lors de la construction de l’image avec la commande ``docker build``, comme présentée précédemment.

- Utiliser la commande ``docker builder prune`` pour vider le cache de Docker.

&nbsp;

Maintenant que nous en savons d’avantage sur le fonctionnement interne de Docker, nous pouvons voir d’autres instructions qui nous permettrons d’apporter plus de flexibilité à nos Dockerfile.

&nbsp;

&nbsp;

## Plus d’instructions pour les Dockerfiles

&nbsp;

Nous avons déjà vu dans la première section les instructions ``FROM``, ``COPY`` et ``ENTRYPOINT``. C’est un bon début mais ces instructions offrent un nombre de possibilités limitées pour nos DockerFile. Comment exécuter une commande shell dans notre conteneur par exemple ?

Dans la section précédente, nous avons vu un aperçu de la commande ``RUN`` qui permet de répondre à ce besoin. Nous avons également évoquer la commande ``ENV`` qui permet de configurer l’environnement du futur conteneur.

&nbsp;

Voyons en détail la syntaxe de ces instructions, et d’autres encore, afin d’exploiter tout le potentiel des DockerFile.

&nbsp;

#### RUN :

L'instruction ``RUN`` peut utiliser la forme « shell » et la forme « exec ».

&nbsp;

<u>Forme « shell » :</u>

```
RUN <command>
```

&nbsp;

<u>Forme « exec » :</u>

```
RUN ["executable", "param1", "param2"]
``` 

&nbsp;

On utilise l'instruction ``RUN`` pour exécuter des commandes dans le shell disponible à l'intérieur de l’image Docker. Il faut bien comprendre que les commandes sont exécutées <u>au moment où l'on construit l’image</u>, et non au démarrage du conteneur.

&nbsp; 

Voici un exemple d’utilisation :

```
RUN ["/bin/bash", "-c", "echo hello"]
```

&nbsp;

Il existe des options spécifiques à l’instruction ``RUN`` qu’il vous faudra explorer directement dans la [référence des Dockerfile](https://docs.docker.com/engine/reference/builder/#run).

&nbsp;

#### CMD :

Comme l'instruction ``RUN``, l'instruction ``CMD`` peut utiliser les deux formes de commande.

&nbsp;

Forme « shell » : 
```
CMD command param1 param2
```
&nbsp;

Forme « exec » : 
```
CMD ["executable","param1","param2"]
```
&nbsp;

La particularité de l’instruction ``CMD`` est qu’il ne peut n'y en avoir qu’une seule par Dockerfile. Il y a une raison à cela : L’instruction ``CMD`` est utilisée pour fournir des arguments par défaut à prendre en compte lors du démarrage du conteneur. Elle peut également spécifier la commande utilisée lors du démarrage. Dans le cas contraire, il est nécessaire de rajouter à la suite une instruction ``ENTRYPOINT`` qui permettra de spécifier l’exécutable à démarrer, l’instruction ``CMD`` est utilisée dans ce cas uniquement pour passer des arguments à l’instruction ``ENTRYPOINT``.
 
&nbsp;

Voici un exemple d’utilisation :
```
CMD ["/usr/bin/wc","--help"]
``` 

&nbsp;

#### WORKDIR :

L’instruction ``WORKDIR`` est plutôt simple d’utilisation. Elle est utilisée pour définir le répertoire de travail au sein de l'image à construire pour les instructions Dockerfile suivantes. Les instructions directement impactées par l’utilisation de l’instruction ``WORKDIR`` sont ``RUN``, ``CMD``, ``ENTRYPOINT``, ``COPY`` et ``ADD``.

&nbsp; 

L’instruction ``WORKDIR`` prend la forme suivante :
```
WORKDIR /path/to/workdir
```

&nbsp;

Voici un exemple d’utilisation :
```
WORKDIR /src
```

&nbsp;

#### ENV :

L’instruction ``ENV`` permet de déclarer une variable d’environnement à l’intérieur de l’image et de lui affecter une valeur. Bien entendu, cette variable pourra être utilisée par les instructions suivantes.

&nbsp; 

L’instruction ``ENV`` prend la forme suivante :
```
ENV <key>=<value>
```

&nbsp;

Voici un exemple d’utilisation :
```
ENV APP_NAME="MyApp"
```

&nbsp;

Il est possible de créer plusieurs variables avec une même instruction ``ENV``.

Il existe également une syntaxe alternative qui omet le signe « = ». Cette approche n’est pas recommandée, elle rend l’instruction moins lisible et ne permet pas de créer plus d’une variable par instruction ``ENV``.

&nbsp;

#### EXPOSE :

L’instruction ``EXPOSE`` permet de définir un port d’écoute pour le futur conteneur. Il est possible de spécifier le protocole pour l’écoute de port (TCP ou UDP) par défaut la valeur choisie est TCP.

Cette instruction ne rend pas le port du conteneur accessible pour autant. En réalité, cette instruction sert plutôt de documentation pour la personne qui va mettre en route le conteneur. C’est l’option -p de la commande « docker run » qui permettra d’exposer le port du conteneur.

&nbsp;

L’instruction ``EXPOSE`` prend la forme suivante :
```
EXPOSE <port>[<port>/<protocol>...]
```

&nbsp;

Voici un exemple d’utilisation :
```
EXPOSE 80/tcp
```

&nbsp;

#### VOLUME :

L’instruction ``VOLUME`` permet de spécifier un point de montage pour un volume qui sera marqué comme un volume monté en externe depuis l’hôte du conteneur ou depuis un autre conteneur.

&nbsp;

L’instruction ``VOLUME`` peut prendre la forme suivante :
```
VOLUME ["/volume-path"]
```
Mais aussi la forme :
```
VOLUME /volume-path
```

&nbsp;

Voici un exemple d’utilisation :
```
VOLUME /myvol
```

&nbsp;

Il est possible de créer plusieurs volumes avec une seule instruction ``VOLUME``.

&nbsp;

Lorsque le conteneur est créé à l’aide de la commande ``docker run``, le ou les volume(s) sont créés par la même occasion, avec comme contenu le répertoire accessible à leur emplacement respectif.

&nbsp;

#### ADD :

L’instruction ``ADD`` est similaire à l’instruction ``COPY``, mais elle offre des fonctionnalités supplémentaires (par ailleurs cette instruction est plus ancienne que l'instruction ``COPY`` !). Pourquoi avoir deux instructions qui fonctionnent de la manière ? Eh bien car l’une est bien plus fournie en fonctionnalité, mais aussi plus complexe. Là où l’instruction ``COPY`` ne peut que copier des fichiers d’un emplacement de l’hôte vers l’image, l’instruction ``ADD`` a en plus la possibilité :

- De récupérer des fichiers depuis une URL pour les ajouter à une image.

- De dézipper une archive et placer son contenu dans la nouvelle image.

&nbsp;

On voit ici l’intérêt que peut apporter l'instruction ``COPY`` sur l'instruction ``ADD`` : si l’on souhaite copier une archive dans l’image, alors on choisira ``COPY``. Au contraire si l’on souhaite dézipper et copier le contenu de l'archive dans l’image, alors on choisira l’instruction ``ADD``.

&nbsp;

L’instruction ``ADD`` prend la forme suivante :
```
ADD [--chown=<user>:<group>] ["<source>",... "<destination>"]
```

&nbsp;

Comme pour la l’instruction ``COPY`` nous avons les arguments « source » et « destination » à spécifier.

``<source>`` est un emplacement du système qui comporte les fichiers à copier.

``<destination>`` est un emplacement dans le système de fichier du futur conteneur.

&nbsp; 

Et comme pour l’instruction ``COPY`` Il est possible de spécifier un utilisateur ou un groupe qui aurait des droits spécifiques sur les éléments à copier, les droits s’appliquant dans le système de fichier du futur conteneur.

&nbsp;

Voici un exemple d’utilisation :
```
ADD home/* /mydir/ 
```

&nbsp;

Nous venons de voir beaucoup de nouvelles instructions, et avec celles-ci vous devriez être en mesure de créer toutes les images Docker nécessaires au développement, aux tests et à la mise en production. Toutefois, n’hésitez pas à consulter la documentation pour approndir vos connaissances sur Dockerfile, et les quelques instructions qui n’auront pas été présentées ici.

&nbsp;

Il est temps désormais de pratiquer ! Dans la prochaine section nous réaliserons un exercice guidé sur la création d’un Dockerfile et la création d’un conteneur à partir d’une image Docker, elle-même générée grâce à notre Dockerfile.