# Exercice pas à pas : initialisation du workflow back-end

Dans cette partie, nous allons réaliser l'initialisation de notre premier workflow via Github Actions qui s'appliquera à la partie back-end de notre solution.
Cet exercice sera effectué pas à pas avec l'appui de captures d'écran vous permettant de suivre la progression.  

## Initialisation du repo Github
Si ce n'est pas déjà fait et afin d'effectuer cet exercice, il est nécessaire d'effectuer un Fork du repository Github suivant : [a-chatelard/FYC-dock-co (github.com)](https://github.com/a-chatelard/FYC-dock-co)  
Pour se faire, il faut accéder à la page du repository puis appuyer sur le bouton "Fork" présent en haut à droite de la page :

![exo](../../images/CICD/init/image%20(3).png)  

Un formulaire de création de fork apparaitra. Vous pouvez nommer votre fork comme vous le souhaitez.  
Il est recommandé de décocher l'option "Copy the main branch only" afin de conserver les autres branches du repository comprenant notamment les corrections.  
Cliquez sur le bouton "Create fork" afin de terminer la création.  

![exo](../../images/CICD/init/image%20(4).png)   

Vous obtiendrez un copie du repository dans laquelle nous travaillerons ensuite.  

![exo](../../images/CICD/init/image%20(7).png)   


À noter que ce repository servira de base à l'ensemble des exercices présents dans cette partie "Partie 5 - Docker et CI/CD". Ces exercices incrémenteront une nouvelle fonctionnalité à notre CI/CD tout au long des chapitres de cette partie.  


## Initialisation du workflow back-end
Maintenant que nous avons à disposition un repository Github sur lequel nous pourrons implémenter notre CI/CD librement, nous pouvons commencer l'initialisation du workflow.  

Cela se passe dans l'onglet "Actions" visible sur les pages de votre repository.  

![exo](../../images/CICD/init/image%20(9).png)   

Il existe de nombreux templates de CI/CD sur lesquels nous pouvons nous appuyer pour mettre en place le notre.  
Cependant, pour minimiser les confusions et rester dans la cadre de la simple initialisation du workflow, nous allons créer notre propre workflow à partir de zéro.  
Pour cela, cliquez sur le bouton "set up a workflow yourself" 


![exo](../../images/CICD/init/image%20(11).png)   

Cette page comprend de nombreux éléments que nous allons revoir ensemble :  


Premièrement on remarque en haut à droite un chemin et un nom de fichier qui représente notre fichier de workflow.  
![exo](../../images/CICD/init/image%20(13).png)   

Le préfixe _"racine/.github.workflow/"_ est obligatoire afin que Github puisse atteindre et reconnaitre les différents fichiers représentant nos différents workflow.  
Étant donné que notre workflow concerne seulement la partie back-end de notre solution nous pouvons par exemple renommer ce fichier "workflow-backend.yaml".  
Pour rappel, il est possible d'affecter n'importe quel nom à ce fichier mais il doit obligatoirement avoir les extensions ".yml" ou ".yaml". Cette dernière étant la plus recommandée parmi les deux.  
![exo](../../images/CICD/init/image%20(21).png)   


Deuxièmement, vous pouvez remarquer en haut à droite de la page, la présence d'un bouton "Start commit" et "Cancel changes".  
![exo](../../images/CICD/init/image%20(15).png)    

Étant donné que nous nous trouvons dans un environnement Git, chaque modification de notre fichier doit être ajoutée à l'historique Git.  
Outre le fonctionnement Git, effectuer des commits réguliers lors de l'ajout de nouveaux éléments dans des fichiers de workflow permet aussi de mieux tracer l'apparition de bugs et de les corriger plus facilement.  

Il vous est donc recommandé d'effectuer régulièrement des commits tout au long des exercices pour éviter les problèmes et faciliter leur réalisation.  


Troisièmement, sur la partie droite de la page, vous trouverez deux onglets "Marketplace" et "Documentation".  
![exo](../../images/CICD/init/image%20(16).png)    

L'onglet "Marketplace" comprend un ensemble de templates de mise en place de workflow. Ces templates comprennent souvent un exemple de code documenté ainsi qu'une brève description des actions effectuées.  
L'onglet "Documentation" quant à lui comprend des exemples basiques des différentes parties d'un fichier de workflow.  


Et enfin, dans la partie centrale de la page, vous pouvez retrouver l'éditeur de fichier de workflow sur lequel nous travaillerons ensuite.  
Vous pouvez, via le raccourci 'ctrl + espace', afficher des suggestions de complétion automatique. Ces dernières comprennent également un brève explication de la balise.  
![exo](../../images/CICD/init/image%20(17).png)    

Il est également possible créer et éditer manuellement un fichier workflow dans votre dossier local sans passer par l'éditeur directement présent dans Github. Faites selon vos préférences.


## Début de la rédaction
Débutons maintenant la rédaction de notre fichier de worflow.  
Pour rappel, nous travaillons sur des fichiers au format Yaml intégrant une syntaxe particulière. Faites donc attention à l'incrémentation de votre code définissant l'arborescence.  

Pour commencer, nous pouvons donner un nom à notre workflow. Ce dernier apparaitra dans l'onglet de gestion Actions lorsque nous aurons fini sa rédaction.  

```name: CI/CD backend```` 
Il est ensuite nécessaire de définir le trigger de notre workflow. Pour rappel, ce trigger représente l'évènement qui servira de déclencheur à notre workflow.  

Il existe de nombreux triggers que vous pouvez retrouver sur la page de documentation officielle.  
Le trigger que nous allons définir sera donc lors push sur notre branche principale :  

Notre objectif final est d'exécuter ce workflow lors de l'ajout d'une fonctionnalité sur la branche principale afin d'enclencher une livraison d'une nouvelle version de notre code sur Dockerhub.  

```
on:
    push:
        branches:
            - main
```  
Dans l'état, ce trigger fonctionnerait. Cependant, nous souhaitons que ce workflow s'enclenche seulement lors d'une modification de la partie back-end. Nous souhaitons également détecter les modifications comprises dans notre fichier de workflow afin d'appliquer les dernières mises à jour de ce dernier.  
Pour se faire, nous pouvons ajouter une balise "paths" limitant le cadre de détection :  
```
on:
    push:
        branches:
            - main
        paths:
            - 'BACK/API.Library/**'
            - '.github/workflows/workflow-backend.yaml'
```
Via ce bloc de code, nous avons défini que le workflow s'enclenchera seulement lors de la modification du code présent dans le dossier "BACK/API.Library" et dans le fichier de "workflow-backend.yaml".  

Si vous avez suivi toutes les étapes, le fichier obtenu devrait comprendre le code suivant :  

```
name: CI/CD backend
on:
    push:
        branches:
            - main
        paths:
            - 'BACK/API.Library/**'
            - '.github/workflows/workflow-backend.yaml'
```
NB : Si vous souhaitez mettre en place des workflows sur un projet de développement concret, il existe de nombreuses manières de définir les triggers des workflows. L'une des plus rependue est [Gitflow](https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow).  

Effectuons un premier commit pour marquer ces différents ajouts.  
Vous pouvons directement effectuer un commit via l'interface Github Actions ou effectuer le commit en local via les commandes Git classiques. Dans ce dernier cas, veillez à bien push ce commit sur le repository distant.   
![exo](../../images/CICD/init/image%20(18).png)    

Si on retourne ensuite dans l'onglet Actions, on remarque que notre workflow apparait mais est en échec.   
![exo](../../images/CICD/init/image%20(19).png)  

Vous pouvez consulter les détails du workflow en cliquant dessus.  
On observe l'erreur suivante :  
![exo](../../images/CICD/init/image%20(20).png)  

Effectivement, de la même manière qu'un fichier docker compose doit comprendre minimum un service, un fichier de workflow Github Actions doit comprendre minimum un job (ou "travail").

Retournons donc dans l'édition de notre fichier de workflow afin de déclarer ces éléments.  

### Ajout des jobs
Comme vu précédemment, notre fichier de workflow ne peut dans l'état pas fonctionner car nous devons y définir au moins un job.  
Dans le cadre de notre exercice, nous souhaitons seulement déclarer un seul job qui comprendra l'ensemble des éléments composant notre pipeline back-end.  

De ce fait, nous pouvons simplifier la rédaction de ce job en définissant le chemin par défaut utilisé lors de l'exécution de commandes bash.
Pour cela, nous pouvons utiliser les balises defaults et run :  
```
defaults:
    run:
        working-directory: ./BACK/API.Library
```
Le code ci-dessus défini le répertoire par défaut dans lequel se placera notre workflow afin d'exécuter des commandes via la paramètre "run" que nous utiliserons plusieurs fois lors de la rédaction de notre fichier de workflow.  

Ce paramètre est bien évidemment optionnel et nous permet seulement de faciliter la futur rédaction des tâches de notre workflow.  

Déclarons maintenant notre job. Ce dernier comprendra un ensemble de step qui seront effectuées l'une après l'autre :  
```
jobs:
    api-library:
        runs-on: ubuntu-latest
```
Via ces trois lignes, nous avons défini notre premier job nommé "api-library". Ce job sera exécuté sur une système ubuntu dans sa dernière version.
Les jobs sont la plupart du temps exécutés sur des systèmes linux car ces derniers ont l'avantage d'être plus légers que des systèmes windows.  

Définissons ensuite nos steps.  
Une step peut effectuer diverses actions tels que :
- exécuter une action définie dans le même dépôt que le workflow, dans un dépôt public ou dans une image conteneur Docker publiée.
- exécuter des programmes en ligne de commande avec une interpréteur  

Du build de notre solution jusqu'à la publication du code sur docker hub, toutes ces actions seront représentées par des steps.  
Il existe de nombreux paramètres permettant de définir une step, vous pouvez les consulter sur la page de [documentation officielle de Github Actions](http://xn--dans%20le%20mme%20dpt%20que%20le%20workflow%2C%20dans%20un%20dpt%20public%20ou%20dans%20une%20image%20conteneur%20docker%20publie-tfl5b4dp26kjb./).  

Notre première step sera une action de checkout sur notre repository.  Cette action est nécessaire si on souhaite accéder au contenu de ce dernier.
```
steps:
    - name : Checkout repository
      uses: actions/checkout@v3
```

Pour se faire, nous utilisons une [action existante "checkout"](https://github.com/actions/checkout) permettant de récupérer notre repository.  
Il est également recommandé de nommer les différentes steps afin de mieux les identifier et de faciliter la rédaction/relecture du fichier.  

Voici le code après les dernières modifications :  
```
name: CI/CD backend
    on:
        push:
            branches:
                - main
            paths:
                - 'BACK/API.Library/**'
                - '.github/workflows/workflow-backend.yaml'
    defaults:
        run:
            working-directory: ./BACK/API.Library
        jobs:
            api-library:
                runs-on: ubuntu-latest
                steps:
                    - name : Checkout repository
                      uses: actions/checkout@v3
```

Maintenant que nous avons défini un job ainsi qu'une step, nous pouvons retester notre workflow.  
Effectuons un nouveau commit et retournons dans l'onglet Actions. On remarque que notre workflow fonctionne :  
![exo](../../images/CICD/init/image%20(22).png)  

## Build de notre solution
Maintenant que notre workflow fonctionne, nous pouvons ajouter les steps relatives à la configuration de dotnet ainsi qu'à l'exécution du build de notre solution.  
Ces deux actions seront chacune représentées par une step de notre job.  

Pour configurer l'environnement dotnet que nous utiliserons ensuite pour le build, nous allons également utiliser une action existante :
```
- name : Set up environment dotnet
  uses: actions/setup-dotnet@v2
  with:
      dotnet-version: '6.0.x'
          
```
L'action utilisée est [setup-dotnet](https://github.com/actions/setup-dotnet). Cette action permet de définir une environnement de CLI dotnet.  
On remarque la présence d'un paramètre "with". Ce paramètre permet de définir certaines options utilisées par l'action. Dans ce cas, nous définissons la version de dotnet qui sera utilisée par l'action pour initialiser l'environnement de CLI dotnet.  

Notre environnement dotnet étant configuré, nous pouvons enfin définir notre step de build :  
```
- name: Build all projects
  run: dotnet build -c Release
  shell: bash
 ```
Nous exécutons cette fois directement une commande via le paramètre "run".  
Étant donné que nous avons précédemment défini le répertoire de travail par défaut, nous n'avons pas besoin d'indiquer de chemin et le chemin utilisé sera  './BACK/API.Library'.  
De plus, notre objectif est de livrer une nouvelle version de notre solution back-end, c'est pour cela que nous utilisons le paramètre '-c Release' définissant la configuration du build.  

Effectuons à nouveau un commit afin de tester notre workflow.  
Vous pouvez suivre l'exécution des différentes steps en cliquant sur l'exécution d'un workflow :  
![exo](../../images/CICD/init/image%20(23).png)  


L'exécution de notre workflow s'est terminée avec succès :   

![exo](../../images/CICD/init/image%20(24).png)  

Il vous est également possible de tester d'autres scénarios :
- Ajouter une erreur dans le code back-end provoquant une erreur lors du build afin de vérifier que cette étape échoue lors de l'exécution du workflow,
- Ajouter une élément ne provoquant pas d'erreur (tel que du code ou un fichier) afin de vérifier que le build fonctionne,
- Modifier un élément en dehors du scope du workflow (par exemple dans la partie front-end) afin de s'assurer que le workflow ne s'enclenche pas.

## Conclusion
Dans cet exercice nous avons mis en place notre premier workflow via l'outil Github Actions.
Ce dernier s'enclenche lors d'un push sur la branche 'main' de notre repository et plus précisément lors d'une modification dans le répertoire 'BACK/API.Library' ou dans le fichier de workflow relatif au back-end.
Le workflow comprend plusieurs étapes :
- Une step de récupération du projet (checkout)
- Une step de setup de l'environnement CLI Dotnet (setup-dotnet)
- Une step d'exécution du build du projet back-end en mode Release


# Exercice : Création du workflow front-end

La première version du workflow back-end étant terminée, nous pouvons nous pencher sur la partie front-end de notre projet.  


Et c'est ...  
## À vous de jouer !  
Dans cet exercice, vous devrez créer un nouveau workflow s'appliquant cette fois-ci à la partie front-end du projet.  
Sa structure ressemblera à celle que du workflow nous avons rédigé précédemment.  
Nous n'utiliserons cependant pas un environnement dotnet mais environnement NodeJs.  
Les commandes effectuées dans la step build seront également différentes. Si vous les avez oubliées, vous pourrez les retrouver par exemple dans le Dockerfile front-end présent dans les branches de la partie 3.  



La correction est faite dans la [vidéo suivante](https://www.youtube.com/watch?v=vLDzxe7jej8)!

