Cas d'usage n°1 : Exemple API quandl
================

Quandl est une API permettant de récupérer des données économiques internationales par région, pays, entreprise etc...
Ces données peuvent être présentes sous différents formats (JSON, CSV,etc...)


## Prérequis :
- Créer un compte sur Quandl et copier la clé (API key)
- Installer NiFi 1.1.0 (nifi-1.1.1-bin.zip) : https://nifi.apache.org/download.html
- Se rendre dans le répertoire nifi-1.1.1/bin puis exécuter run-nifi.bat (sous windows)
- Ouvrir un navigateur internet puis se rendre à l'adresse http://localhost:8080/nifi/ (cela peut prendre quelques minutes avant que l'interface web s'affiche)

## But de l'exemple :
- Introduire Nifi avec un exemple simple
- Etablir une connexion avec une API pour récupérer des fichiers (JSON, CSV etc...)
- Configurer une connexion SSL pour pouvoir se connecter à une page web sous le protocole https

## Schéma du flow final

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/flow1.JPG)

### Configuration du processeur GetHttp

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/processeur%20icone.PNG)

Faites glisser l'icône ci-dessus sur la partie quadrillée de l'interface pour selectionner un processeur. Dans la partie recherche, tapez "http" puis sélectionnez le processeur "GetHttp". Faites un clic droit, et cliquez sur "properties". La fenêtre suivante devrait apparaître :

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/propri%C3%A9t%C3%A9s%20getHttp.JPG)

Remplissez les informations ci-dessus :

- Filename : nom du fichier (on récupère un fichier json dans ce cas d'où le .json)
- URL : https://www.quandl.com/api/v3/datasets/WIKI/FB.json?api_key="YOURAPIKEY" (remplacez "YOURAPIKEY" par la clé fournie lors de l'inscription)
- Si vous utilisez un proxy (très courant en entreprise), il faut préciser les valeurs de l'hôte et du port.
- Dans certaines API, il faut préciser un nom d'utilisateur (Username) correspondant à la clé de l'API.

En cliquant sur "Apply", on se rend compte qu'un symbole d'erreur apparaît sur le processeur : ![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/warning.PNG)

En passant, la souris dessus, on se rend compte, qu'il faut sélectionner un "SSL context service" afin de pouvoir établir la connexion avec le protocle https.

### Mise en place de la connexion https

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/propri%C3%A9t%C3%A9s%20getHttp.JPG)

Cliquez sur la case "Value" de SSL context service. Le menu déroulant suivant (sans "connexion https") devraît apparaître :

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/create%20new%20service.JPG)

Créez un nouveau service en cliquant sur "create new service". Vous aurez la possibilité de créer un nouveau type de connexion pour votre processeur GetHttp :

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/add%20ControllerService.JPG)

Après avoir confirmé la création, le menu suivant apparaît :

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/props%20ssl.JPG)

Le statut de la connexion est "invalide" puisque la conenxion n'a pas été configurée. Cliquez sur le crayon à droite des propriétés : ![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/crayon%20modif.JPG)

Vous pouvez alors modifier les propriétés du "Context SSL service" selon ce qui est affiché ci-dessous :

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/props%20ssl%202.JPG)

- 

### Configuration du processeur PutFile

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/putfile%20processor%20settings.JPG)

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%201/Images/putFile%20properties.JPG)

### Résultat



