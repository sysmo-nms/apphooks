Simple pubsub + API web
-----------------------

Plus simple que la version 1. Une API web complete avec authentification digest.

## Un pubsubhubbub/API web

L'application distante qui s'initialise effectue:
- une souscription subhub,
- prend et traite un dump du topic (probes, targets) demand� au serveur (via API Web),
- commence � traiter les evenements subhub.

D�s lors il est synchro.

## Une API web
Chaque modification/supr�ssion/cr�ation d'�l�ment d�clanche un �v�nement.
Quand une application usilise l'API web pour modifier un �l�ment l'evenement contient le nom de l'auteur de la modification (permet � l'application d'ignorer les evenements d�clanch� par lui m�me).


## Comportement particulier
Le serveur retourne succes si la ressource � modifier n'existe pas (evenement d�l�tion certainement en transit),

## D�tail de l'API Web
- ajouter/supprimer sonde,
- ajouter/supprimer cible,
- modifier propri�t� sonde,
- modifier propri�t� cible (ex:url access ressource),
- pose/d�marer sonde


## D�tail de l'utilisation des propri�t�s cot� serveur
Voir la DEFINITION.md pour le d�tail, en particulier la fonction d'acces � la ressource depuis l'url configur�e par l'application.


*Et voila!*

