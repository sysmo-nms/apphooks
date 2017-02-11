Simple pubsub + API web
-----------------------

Plus simple que la version 1. Une API web complete avec authentification digest.

## Un pubsubhubbub/API web

L'application distante qui s'initialise effectue:
- une souscription subhub,
- prend et traite un dump du topic (probes, targets) demandé au serveur (via API Web),
- commence à traiter les evenements subhub.

Dés lors il est synchro.

## Une API web
Chaque modification/supréssion/création d'élément déclanche un évènement.
Quand une application usilise l'API web pour modifier un élément l'evenement contient le nom de l'auteur de la modification (permet à l'application d'ignorer les evenements déclanché par lui même).


## Comportement particulier
Le serveur retourne succes si la ressource à modifier n'existe pas (evenement délétion certainement en transit),

## Détail de l'API Web
- ajouter/supprimer sonde,
- ajouter/supprimer cible,
- modifier propriété sonde,
- modifier propriété cible (ex:url access ressource),
- pose/démarer sonde


## Détail de l'utilisation des propriétés coté serveur
Voir la DEFINITION.md pour le détail, en particulier la fonction d'acces à la ressource depuis l'url configurée par l'application.


*Et voila!*

