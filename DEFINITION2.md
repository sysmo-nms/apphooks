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
- Le serveur retourne succes si la ressource à modifier n'existe pas (evenement délétion certainement en transit),

## En cas de problème réseau
Trois solutions
1 - Le coté serveur accumule les évènements pour tous les envoyer en même temps des que la connexion est rétablie (prevoir un timeout),
2 - Les évènements possèdent un identifiant incrémentable si l'application detecte une anomalie: se déconnecter et recommencer (subhub,dump, le plus simple),
3 - Les évènements possèdent un identifiant incrémentable si l'application detecte une anomalie: un journal d'evenements coté serveur (l'application demande les évènements manquants).

Si journal serveur: 
- limiter la durée de rétention à 24H, et unsubscribe l'application,
- check alive depuis app->serveur qui retourne erreur si plus souscripteur.

## Détail de l'API Web serveur
- ajouter/supprimer sonde,
- ajouter/supprimer cible,
- modifier propriété sonde,
- modifier propriété cible (ex:url access ressource),
- pose/démarer sonde
- recupérer info sonde/cible,
- recupérer info toutes les sondes,
- recupérer info toutes les cibles,
- id de l'actuel id evenement (pour target/probe),
- ping.

## Détail de l'API Web hook
- nouvel evenements (1..N),
- dump (target/probe),
- ping.


## Détail de l'utilisation des propriétés coté serveur
Voir la DEFINITION.md pour le détail, en particulier la fonction d'acces à la ressource depuis l'url configurée par l'application.


*Et voila!*

