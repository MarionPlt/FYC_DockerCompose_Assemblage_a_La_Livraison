Dans cette courte partie, nous allons effectuer l'installation des outils docker nécessaires au bon déroulement du cours. 
Nous nous appuierons sur la documentation officielle d'installation de docker.

Selon votre système d'exploitation, certaines manipulations supplémentaires seront nécessaires :
- Windows : https://docs.docker.com/desktop/install/windows-install/
- Mac : https://docs.docker.com/desktop/install/mac-install/
- Linux : https://docs.docker.com/desktop/install/linux-install/

La documentation Docker recommande l'installation du logiciel Docker Desktop afin de mettre en place l'ensemble des outils nécessaires à son fonctionnement.
Le logiciel intègre : 
- Un noyau Docker, Docker Desktop doit donc être obligatoirement lancé afin de pouvoir utiliser les différentes commandes Docker,
- Un CLI Docker permettant justement d'effectuer toutes ces commandes,
- Une interface de gestion, permettant de gérer nos ressources Docker,
- Et d'autres outils tels que Docker Compose que l'on utilisera durant cette formation.

Il est également recommandé de vous créer un compte sur [Docker hub](https://hub.docker.com/) directement sur le site ou via Docker Desktop en prévision d'un futur chapitre. 

Spécificité système :
Si vous installez Docker Desktop sur Windows, vous devrez également [installer WSL 2](https://docs.docker.com/desktop/install/windows-install/#wsl-2-backend) (Windows Subsystem for Linux). 
Si vous installez Docker Desktop sur Linux, vous devrez vous assurer que votre système possède le [support KVM](https://docs.docker.com/desktop/install/linux-install/#kvm-virtualization-support) (Kernel Virtual Machine).

Git
Vous aurez également besoin durant cette leçon d'effectuer des commandes Git. Si ce n'est pas déjà fait, vous pouvez installer l'outil via [cette URL](https://git-scm.com/downloads).
