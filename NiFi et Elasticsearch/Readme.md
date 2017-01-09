# Intégration de NiFi avec Elasticsearch
================

## Prérequis :
- Disposer de NiFi 1.1.0 (voir Exemple 1)
- Installer la version 2.4.3 d'Elasticsearch
- Lancer Elasticsearch : aller dans le dossier elasticsearch-2.4.3\bin, puis exécuter le fichier de commande elasticsearch.bat
- Taper l'adresse http://localhost:9200/ (précédé de l'instruction "curl" si vous testez sur un shell linux/Git Bash)
- Ouvrir le fichier elasticsearch-2.4.3\config/elasticsearch.yml et changer le nom du cluster elasticsearch : 
(remplacer "# cluster.name: my-application" par "cluster.name: elasticsearch")

ATTENTION : la version 5.1.1 (récente) d'Elasticsearch ne marche pas avec le processeur PutElasticsearch de NiFi

## But de l'exemple :
- Découvrir comment imbriquer NiFi avec Elasticsearch de manière très simple

## Processeurs utilisés et remarques :
- GetTwitter, EvaluateJSONPath, RouteOnAttribute, UpdateAttribute, PutFile, AttributesToJSON, LogAttribute
- Le processeur GetTwitter ne fonctionne pas si vous disposez d'un proxy !!

Schéma du flow :

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/Dataflow%20tweets%20elastic.JPG)

### Configuration du processeur GetTwitter

On obtient tous les tweets mentionnant un des quatre termes suivants : nifi,hadoop,hortonworks,elasticsearch

Propriétés :
- Consumer Key , Consumer Secret, Access Token, Access Token Secret : informations disponibles sur https://apps.twitter.com/ (il faut créer une application twitter et obtenir ces informations)
- Terms to filter on : nifi,hadoop,hortonworks,elasticsearch
- Ce processeur ne marche pas si vous utilisez un proxy

### Configuration du processeur EvaluateJsonPath

On extrait les attributs qui nous intéressent avec ce processeur (message, coordonnées géographiques, langue, identifiant utilisateur, etc...)

### Configuration du processeur RouteOnAttribute

On rajoute la propriété "tweet" (bouton "+" ) vérifiant que le tweet reçu contient du texte. Si ce n'est pas le cas, on ne transmet pas le fichier au processeur suivant. 

### Configuration du processeur PutElasticsearch

![alt tag](https://github.com/DataWarehouse2017/Tests-NiFi/blob/master/NiFi%20et%20Elasticsearch/screenshotsElasticsearch/ConfigurePutElasticSearch.PNG)
- index : index sur dans lequel les documents seront stockés. S'il n'existe pas, Elasticsearch le crée.
- Identifier Attribute : identifiant du document. Il est unique. On choisit donc uuid qui est l'identifiant unique d'une unité de fichier (ici sous format JSON) dans NiFi
- ATTENTION : il faut remplacer le port 9200 par le port 9300 pour renseigner l'hôte d'Elasticsearch comme spécifié sur les propriétés ci-dessus.

### Résultat

On obtient bien plusieurs tweets stockés dans l'index "tweets".



