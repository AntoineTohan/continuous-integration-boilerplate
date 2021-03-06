# Intégration continue boilerplate

***Rédiger par DURAND Antoine, THOMAS Hugo, JAMMES Yannick et SACHE Mickael.***

## SOMMAIRE

+ [Objectifs](#objectifs)
+ [Installer Docker](#installer-docker)
+ [Préparer l'environnement](#préparer-l'environnement)
+ [GitLab configuration](#gitlab-configuration)
+ [SonarQube configuration](#SonarQube-configuration)
+ [Jenkins configuration](#Jenkins-configuration)
+ [Ajouter une application Java à Jenkins](#Ajouter-une-application-Java-à-Jenkins)
+ [Maintenir son environnement](#Maintenir-son-environnement)
+ [Pour aller plus loin](#Pour-aller-plus-loin)
+ [Troubleshooting](#Troubleshooting)

## OBJECTIFS

Avoir un environnement de CI opérationnel en quelques minutes et sans polluer son OS.
Le but et de mettre en place via docker un outil de virtualisation de 3 serveur en place sur votre machine : un serveur Jenkins , un serveur SonarQube et un serveur Gitlab. Avec cet environnement il vous seras facile d’adapter la structure pour intégrer de la CI dans vos projets quelconques (NodeJs, Java).

Le schéma de l’environnement que nous allons mettre en place : 

![archi](images/archi.png)

## INSTALLER DOCKER

Pour installer Docker sur Windows, il faut obligatoirement Windows 10 professionnel, sinon il faudra télécharger Docker ToolBox.

Pour l’installation de Docker ou Docker ToolBox il est nécessaire d’avoir accès à un terminal de ligne de commande type “shell bash Linux”.  Pour linux et Mac, utiliser le terminal natif. Si vous êtes sur Windows et qu’il ne l’embarque pas déjà, suivez le guide suivant ou équivalent : https://www.zebulon.fr/astuces/divers/executer-linux-sous-windows-10.html.

Si vous ne pouvez pas installer le shell bash pour Windows, installer et utiliser le bash github : https://git-scm.com/download/win.

**Windows 10 PRO :**
- Ce rendre sur le site de Docker : https://www.docker.com/get-started
- Cliquer sur le bouton “**Download Desktop and Take a Tutorial**”
- Créer un compte Docker
- Télécharger l'exécutable pour Windows 
- Double-cliquez sur **"Docker Desktop Installateur.exe**" pour lancer l'installateur
- Suivez l'assistant d'installation : acceptez la licence, autorisez l'installateur et procédez à l'installation
- Cliquez sur "**Close**" pour terminer l'installation
- Dans un nouveau terminal lancer ```docker -v``` , si le terminal vous donne la version actuelle de docker alors l’installation est réussi

![docker-v](images/docker-v.png)

- Vous pouvez maintenant lancer Docker Desktop en le recherchant dans la barre de recherche de Windows et en cliquant dessus
- Exécuter le tutoriel Docker Hello-World pour apprendre les commandes de bases de docker et vérifier l'installation ```docker run hello-world ```.

**Pour Windows 10 (Standard, Famille..) ou < 10 :**
- Ce rendre sur le GitHub Docker ToolBox : https://github.com/docker/toolbox/releases
- Télécharger le dernier exécutable stable disponible (“**DockerToolbox-XX.XX.X.exe**”)
- Suivre les instructions d’installations du launcher
- Une fois fini vous devriez avoir un nouveau programme “**Docker QuickStart**”
- Exécuter “**Docker QuickStart**”
- Un terminal s’ouvre, s’il demande un “**User Account Control**”, répondre par “**Yes**”
- Quand l’initialisation est terminée, le terminal affichera un “$”
- Dans un nouveau terminal lancer ```docker -v``` si le terminal vous donne la version actuelle de docker alors l’installation est réussi

![docker-v](images/docker-v.png)
- Exécuter le tutoriel Docker Hello-World pour apprendre les commandes de bases de docker et vérifier l'installation.```docker run hello-world``` 

**Pour MacOS:**
- Ce rendre sur le Docker : https://docs.docker.com/docker-for-mac/install/ et cliquer sur “**Download from Docker Hub**”
- Créer un compte Docker
- Exécuter le fichier “**docker.dmg**” 
- Suivre l’installateur comme pour toute application Mac, déplacer l'éxécutable dans le dossier “**Application**”
- Exécuter Docker Desktop
 ![mac-1](images/mac-1.png)
- Vous devriez voir apparaître l'icone de Docker dans la TopBar
![mac-2](images/mac-2.png)
- Cliquer sur l’icone docker de la TopBar
![mac-3](images/mac-3.png)
- Une fenêtre vous affichera le statut d'installation de votre docker et aussi un formulaire pour vous connecter avec votre compte Docker.
- Dans un nouveau terminal lancer ```docker -v``` si le terminal vous donne la version actuelle de Docker, alors l’installation est réussi
![docker-v](images/docker-v.png)
- Exécuter le tutoriel Docker Hello-World pour apprendre les commandes de bases de docker et vérifier l'installation```docker run hello-world```.

**Pour Ubuntu Disco 19.04 / Cosmic 18.10 / Bionic 18.04 (LTS) / Xenial 16.04 (LTS) :**

METTRE EN PLACE LE DÉPÔT

- Ouvrez un terminal natif et updater les packages : ```sudo apt-get update```
- Installer des paquets pour permettre à apt d'utiliser un référentiel sur HTTPS :
```sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent  software-properties-common```
- Ajoutez la clé GPG officiel de Docker :
```curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -```
- Vérifiez que vous avez maintenant la clé avec l'empreinte digitale 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, en recherchant les 8 derniers caractères de l'empreinte digitale.
```sudo apt-key fingerprint 0EBFCD88```
- Utilisez la commande suivante pour mettre en place le dépôt stable :
```sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"```

INSTALLER DOCKER ENGINE - COMMUNITY

- Une fois le dépôt bien ajouté, lancer la commande suivante : ```sudo apt-get update```
- Installer la dernière version de Docker Engine - Community : ```sudo apt-get install docker-ce docker-ce-cli containerd.io```
- Dans un nouveau terminal lancer ```docker -v``` si le terminal vous donne la version actuelle de docker alors l’installation est réussi
![docker-v](images/docker-v.png)
- Exécuter le tutoriel Docker Hello-World pour apprendre les commandes de bases de docker et vérifier l'installation```docker run hello-world```.


INSTALLER DOCKER COMPOSE 
- Cette commande permet de télécharger la version stable actuelle de Docker Compose : ```sudo curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose```
- Appliquer les permissions d'exécution au binaire :
```sudo chmod +x /usr/local/bin/docker-compose```
- Test the installation : 
```docker-compose --version```
Cela devrait afficher : 

![docker-compose-v](images/docker-compose-v.png)


## PRÉPARER L’ENVIRONNEMENT

Nous allons démarrer 3 serveurs en même temps grâce à Docker. Le serveur Jenkins, le serveur Gitlab et le serveur SonarQube. Or il faudra que ces trois serveurs puissent communiquer et échanger de données entre eux et avec votre ordinateur aussi. Ainsi pour des soucis de configuration future nous n’allons pas démarrer chaque service les uns après les autres mais exécuter un « docker-compose » qui va démarrer les 3 services en même temps avec la configuration nécessaire. 

Comment démarrer les services :

Tout d'abord, vous devez cloner ce dépôt :
``` git clone https://github.com/AntoineTohan/continuous-integration-boilerplate.git```

***Notez que vous devez installer git sur votre système : https://git-scm.com/***

Naviguez avec votre terminal jusqu'au dossier :
``` cd continuous-intégration-boilerplate```

Vous pouvez maintenant lancer tous les services avec le Docker-compose :
``` docker-compose up -d```

Documentation des compose file : https://docs.docker.com/compose/compose-file/

Lister les containers en running status :
``` docker ps```

Liste des containers créée :
![docker-ps](images/docker-ps.png)


## GitLab configuration

**Qu'est ce que Gitlab ?**

GitLab est un logiciel libre basé sur git proposant les fonctionnalités de wiki, un système de suivi des bugs, l’intégration continue et la livraison continue.

- Accès à l'interface web de gitlab à l'adresse suivante : http://localhost:8081

- Entrez un mot de passe pour le compte root
- Vous pouvez maintenant vous connecter avec le nom d'utilisateur : root et votre mot de passe
- Une fois connecté, vous devrez autoriser le GitLab à demander un réseau local afin de permettre la communication entre le GitLab et Jenkins
- Allez dans les paramètres (uniquement accessible avec le compte root)

![gitlab-1](images/gitlab-1.png)

- Allez ensuite dans la barre latérale gauche  "**Settings** -> **Networks**"

- Dans le menu "**Outbound requests**" cochez les deux cases : 
![gitlab-2](images/gitlab-2.png)

- Vous pouvez maintenant utiliser Gitlab et créer les projets dont vous avez besoin.

## SonarQube configuration

**Qu'est ce que SonarQube ?**

SonarQube est un logiciel libre permettant de mesurer la qualité du code source en continue.

- Accès à SonarQube à l'adresse : http://localhost:9000

- Cliquez sur "**Login**". Le nom d'utilisateur et le mot de passe par défaut sont ```admin```.

- Par défaut, SonarQube ne peut analyser aucun code source car aucun plugin n'est installé. Il faut donc installer les plugins adaptés.

Cliquez sur "**Administration**" et "**Marketplace**" , et installer le plugin "SonarJava" :

![sonar-1](images/sonar-1.png)

***Dans l'exemple ci dessus on ajoute Java***

- Exemple d'ajout d'un projet Java (Maven ou Graddle) : 

- Créez un nouveau projet, cliquez sur "**Create new project**": 
![sonar-2](images/sonar-2.png)

- Nommez votre projet : 
![sonar-3](images/sonar-3.png)

- Générez un token, ajouter un nom et cliquer sur "**Generate**" : 

![sonar-4](images/sonar-4.png)

- Sélectionnez "**Java**" , puis "**Maven ou Graddle**" selon votre projet : 
![sonar-6](images/sonar-6.png)

- Copiez bien la commande  donner par SonarQube pour lier votre projet au SonarQube.
![sonar-7](images/sonar-7.png)

Il vous suffira ensuite d'aller à la racine de votre projet Java avec un terminal et de lancer la commande copiée précédemment. Vous devriez avoir un message : 

![sonar-8](images/sonar-8.png)

***Pour lancer une analyse SonarQube dans votre projet il vous suffira de lancer la commande ```mvn sonar:sonar```à la racine du projet concerné.***

## Jenkins configuration

**Qu'est ce que jenkins ?**

Jenkins est un outil d'intégration continue open source écrit en Java. Le projet a été conçu à partir de Hudson après un conflit avec Oracle.
Jenkins fournit des services d'intégration continue pour le développement de logiciels. Il s’agit d’un système serveur fonctionnant dans un conteneur de servlet tel qu'Apache Tomcat.


- Accèder à  Jenkins à l'adresse suivante : http://localhost:8080
- Une fenêtre apparaît vous demandant de déverrouiller Jenkins, pour trouver le mot de passe il faut  :

  - Par le terminal : 
    - ```docker ps``` copy container ID of jenkins. 
    - ```docker logs <ContainerID>``` and search the admin password generate in logs.

  - Directement dans votre explorateur de fichiers :
    - Allez à la racine du projet -> jenkins -> secrets -> initialAdminPassword
    ![jenkins-1](images/jenkins-1.png)
- Ensuite, cliquez sur "**Installer les plugins suggérés**" et attendez la fin du processus
- Vous pouvez maintenant créer votre compte d'administration et cliquer sur "**Save and Continue**"
- Laissez ensuite l'URL Jenkins suggérée et cliquez sur "**Save and Finish**". Pour permettre à Jenkins de communiquer avec GitLab et SonarQube, nous devons installer quelques plugins.

**Configuration du plugin SonarQube**

- Cliquez sur "**Manage Jenkins**" et ensuite sur "**Manage Plugin**"
- Maintenant, recherchez "**SonarQube Scanner**" et cochez la case du plugin SonarQube Scanner, puis cliquez sur "**Download now and install after restart**".
![jankins-2](images/jenkins-2.png)


- Ensuite cliquer sur "**Restart Jenkins when installation is complete and no jobs are running**"
- Attendez le redémarrage de Jenkins

- Allez dans SonarQube (disponible à l'adresse suivante : localhost:9000)
- Cliquez sur votre profil en haut à droite, puis "**My Account**"

![sonar-account](images/sonar-account.png)

- Cliquez dans la section "**Security**"
- Ajouter un nouveau Token et copier la clé secrète fournie par SonarQube
- Retournez sur Jenkins
- Pour la configuration de SonarQube allez dans "**Administrer Jenkins**" puis "**Configurer le système**"
- Ajouter un serveur SonarQube avec la configuration suivante : 

![sonar-scanner-conf](images/sonar-scanner-conf.png)

  - **Nom** : sonar-scanner
  - **URL du serveur** : http://sonarqube:9000/
  - Cliquez sur le bouton "**Ajouter**" du champ "**Server authentification token**"

![jenkins-add-sonar-token](images/jenkins-add-sonar-token.png)

  - Ouvrez le champ "**Type**" et sélectionner "**Secret Text**"
  - Dans le champ "**Secret**", entrez la clé secrète SonarQube précédemment copiée
  - Dans le champ "**ID**", rentrez "**sonar-scanner-token**"
  -  Laissez le champ "**Description**" vide
  - Cliquez sur le bouton "**Ajouter**"

![jenkins-token](images/sonar-tokenpng.png)

  - Maintenant, sélectionner dans "**Server Authentification Token**" le token précédemment crée

- Rendez-vous dans l'espace suivant : "**Administrer Jenkins** --> **Configuration globale des outils**"
  - Dans la catégorie "**SonarQube Scanner**" cliquez sur "**Ajouter SonarQube Scanner**"
  - Dans le champ "**Name**", écrire : "**sonar-scanner**"
  - Cochez "**Install Automatically**"

![sonar-scanner-jekins-outils](images/sonar-scanner-jekins-outils.png)

L'installation et la configuration du SonarQube plugin est désormais terminée.

**Installation et configuration du plugin Maven**

-  Cliquez sur "**Manage Jenkins**" et ensuite sur "**Manage Plugin**"
-  Recherchez et ajoutez le plugin "**Maven Integration Plugin**"

![maven-integration-plugin](images/maven-integration-plugin.png)

-  Redémarrez Jenkins quand l'installation est terminée
-  Allez dans "**Administrer Jenkins**" puis "**Configuration globale des outils**"
- Dans la section "**Maven**" cocher **"Install Automatically**"
- Sélectionner "**Download from Docker.com**"
- Enregistrez et appliquez les changements

![maven](images/maven.png)

Maven est désormais installé et ne nécessite pas plus de configuration
 
## Ajouter une application Java à Jenkins

- Rendez-vous à la page d'accueil de Jenkins
- Cliquez sur "**New Item**"
- Sélectionner "**Construire un projet Maven**"
- Dans "**Nom**", saisissez un nom de projet
- Cliquez sur le bouton "**Ok**"

![jenkins-create-jobs](images/jenkins-create-jobs.png)

Votre build Jenkins est désormais créée.

Nous allons maintenant configurer le build Jenkins

- Dans la catégorie "**Gestion de code source**", ajoutez le lien vers votre repository Github (vous devrez ajouter des credentials si votre repository est privé)
- Dans la catégorie "**Ce qui déclenche le build**", cochez "**Scrutation de l'outils de gestions de version**"
  - Cocher l'option "**Lance un build à chaque fois qu'une dépendance SNAPSHOT est construite**" 
  - Dans "**Planning**", rentrez une valeur pour que Jenkins aille scruter votre repository toutes les X minutes (* * * * * = toutes les minutes)

![jenkins-conf-1](images/jenkins-conf-1.png)

- Dans la catégorie "**Post Step**", sélectionner "**Lancer une analyse avec SonarQube Scanner**"
  - Cocher l'option "**Run only if build succeeds**"
  - Dans "**Propriétés de l'analyse**", ajoutez le code suivant, en modifiant pour votre projet :
```sonar.projectKey=dadCooking-front
sonar.projectKey=project_name 
(remplacez par le nom de votre projet)
sonar.sources=src/main
sonar.sourceEncoding=UTF-8
sonar.language=java
sonar.tests=src/test
sonar.junit.reportsPath=target/surefire-reports
sonar.surefire.reportsPath=target/surefire-reports
sonar.jacoco.reportPath=target/jacoco.exec
sonar.java.binaries=target/classes
sonar.java.coveragePlugin=jacoco
```

![jenkins-conf-1](images/jenkins-conf-2.png)

- Enregistrer la configuration de votre build

Votre build est maintenant configuré.

Si vous le lancer votre pipeline :

![launch-jenkins-build](images/launch-jenkins-build.png)

Vous devriez avoir :

![jenkins-jobs-finish](images/jenkins-jobs-finish.png)

Voici la liste des étapes de votre jobs jenkins :
- Maven Build
- Maven TestUI
- Génération des rapports de test
- Génération des artefacts JAR
- Analyse du code SonarQube

Les commandes Build, Test et la génération des tests et artifacts et géré par le plugin "Maven Integration plugin".

##  Maintenir son environnement

Si vous avez suive la totalité des étapes précédantes vous devriez avoir un server SonarQube, Jenkins et Gitlab fonctionnel. Avec cette architecture vos serais en mesure de pouvoir appeler des analyse SonarQube depuis Jenkins et déclencher des builds Jenkins depuis une actions Gitlab (commit / PR etc ....). 

Pour terminer quelques commandes afin de mettre en pause / supprimer tous l'environnement d'intégration continue mis en place.

Naviguez avec votre terminal jusqu'au dossier cloner depuis github :
```cd continu-intégration-boilerplate```

Vous pouvez maintenant mettre en pause les containers avec  :
``` docker-compose stop```

Et vous pouvez redémarrer les containers avec :
``` docker-compose start```

Vous pouvez maintenant supprimer tous les services avec le Docker-compose :
***Attention la suppression vous fera perdre toute votre configuration***
``` docker-compose down```

## Pour aller plus loin

- Afin d'automatiser les builds et les analyses SonarQube, vous pouvez créer des JenkinsFile & des fichiers de configuration SonarQube. 
- Pour créer des JenkinsFile : https://jenkins.io/doc/pipeline/examples/
- Pour créer des Sonar Properties : https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/
- Aussi, si vous ne souhaitez pas passer par Jenkins, Gitlab propose lui aussi un système d'intégration continiue : https://docs.gitlab.com/ee/ci/

## Troubleshooting
***Si vous rencontrez des problémes lors des installations des outils, vous pouvez vous référez aux GitHub/FAQ suivantes :***

- Pour Docker : https://docs.docker.com/ | https://github.com/docker/for-mac | https://github.com/docker/for-win
- Pour Gitlab : https://docs.gitlab.com/
- Pour Jenkins : https://jenkins.io/doc/ | https://github.com/jenkinsci/jenkins
- Pour SonarQube : https://docs.sonarqube.org/latest/ | https://github.com/SonarSource/sonarqube

***Si ces liens ne vous aident pas, vous pouvez toujours ouvrir une "issue" sur ce Github et nous vous répondrons.***
