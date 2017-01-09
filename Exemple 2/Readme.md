Cas d'usage n° 2 : Analyse de tweets
================

Pour tester, vous pouvez télécharger le template et l'importer sur NiFi (Analyse_et_filtrage_tweets.xml)

## Prérequis :
- Disposer d'un compte twitter et créer une application twitter sur https://apps.twitter.com/
- Disposer de NiFi 1.1.0 (voir Exemple 1)

## But de l'exemple :
- Récupérer et trier les tweets en fonction des candidats à la présidentielle mentionnés dans le message
- Utiliser les fonctionnalités de certains processeurs sur NiFi
- Découvrir le langage d'expression de NiFi
- Appronfondir l'Exemple 1

## Processeurs utilisés et remarques :
- GetTwitter, EvaluateJSONPath, RouteOnAttribute, UpdateAttribute, PutFile, AttributesToJSON, LogAttribute
- Le processeur GetTwitter ne fonctionne pas si vous disposez d'un proxy !!

## Schéma du flow final

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/flow.PNG)

### Configuration du processeur GetTwitter

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/ConfigureGetTwitter.PNG)

On obtient tous les tweets mentionnant un des quatre candidats à la présidentielle (François Fillon, Emmanuel Macron, Marine le Pen et Jean Luc Melenchon).

Propriétés :
- Consumer Key , Consumer Secret, Access Token, Access Token Secret : informations disponibles sur https://apps.twitter.com/ (il faut créer une application twitter et obtenir ces informations)
- Terms to filter on : @EmmanuelMacron, @FrancoisFillon, @JLMelenchon, @MLP_officiel
- Ce processeur ne marche pas si vous utilisez un proxy

### Configuration du processeur EvaluateJsonPath

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/ConfigureEvaluateJsonPath.PNG)

On extrait les attributs qui nous intéressent avec ce processeur (message, coordonnées géographiques, langue, identifiant utilisateur)

### Configuration du processeur RouteOnAttribute

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/ConfigureRouteOnAttribute.PNG)

On rajoute la propriété "tweet" (bouton "+" ) vérifiant que le tweet reçu contient du texte. Si ce n'est pas le cas, on ne transmet pas le fichier au processeur suivant. 

### Configuration du processeur UpdateAttribute

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/ConfigureUpdateAttribute.PNG)

On rajoute la propriété "candidate" (qui correspondra au candidat mentionné dans le tweet). 
On ajoute également l'attribut "filename" afin de le modifier

- filename : ${twitter.handle}_${twitter.tweet_id:toString()}.json

Cliquez sur le bouton "ADVANCED". Le menu suivant s'affiche :

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/advancedSettingsEmpty)

Créez plusieurs règles qui vont permettre de modifier la valeur de l'attribut "candidate". pour cela, on vérifie si le message du tweet contient une référence à un des candidats :

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/advancedSettingsUpdateAttribute.PNG)

### Configuration du processeur PutFile

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/ConfigurePutFile.PNG)

On souhaite placer les tweets mentionnant chaque candidat présidentiel dans un dossier différent.
On modifie le dossier destination de la façon suivante :

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/directoryPutfile.PNG)

- Directory : C:\Users\Ali\Desktop\Big Data\NiFi\tweets\${candidate}
- ${candidate} : attribut "candidate". Cet attribut a été modifié dans le processeur UpdateAttribute

### Configuration du processeur AttributesToJSON

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/ConfigureAttributesToJSON.PNG)

On génère un nouveau fichier JSON à partir des attributs suivants : 
- twitter.user, candidate, twitter.language, twitter.location, twitter.time, twitter.msg

### Configuration du processeur LogAttribute

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/ConfigureLogAttribute.PNG)

On choisit d'afficher les attributs "candidate" et "Filename" afin de vérifier qu'ils ont bien été modifiés. Afin de pouvoir consulter les fichiers JSON sur l'interface web, on choisit "info" comme façon d'afficher les attributs.

### Résultat

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Exemple%202/screenshots2/resultat.PNG)

On obtient bien plusieurs tweets mentionnant chaque candidat à la présidentielle.


