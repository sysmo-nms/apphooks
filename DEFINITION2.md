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
- Le serveur retourne succes si la ressource � modifier n'existe pas (evenement d�l�tion certainement en transit),

## En cas de probl�me r�seau
Trois solutions
1 - Le cot� serveur accumule les �v�nements pour tous les envoyer en m�me temps des que la connexion est r�tablie (prevoir un timeout),
2 - Les �v�nements poss�dent un identifiant incr�mentable si l'application detecte une anomalie: se d�connecter et recommencer (subhub,dump, le plus simple),
3 - Les �v�nements poss�dent un identifiant incr�mentable si l'application detecte une anomalie: un journal d'evenements cot� serveur (l'application demande les �v�nements manquants).

Si journal serveur: 
- limiter la dur�e de r�tention � 24H, et unsubscribe l'application,
- check alive depuis app->serveur qui retourne erreur si plus souscripteur.

## D�tail de l'API Web serveur
- ajouter/supprimer sonde,
- ajouter/supprimer cible,
- modifier propri�t� sonde,
- modifier propri�t� cible (ex:url access ressource),
- pose/d�marer sonde
- recup�rer info sonde/cible,
- recup�rer info toutes les sondes,
- recup�rer info toutes les cibles,
- id de l'actuel id evenement (pour target/probe),
- ping.

## D�tail de l'API Web hook
- nouvel evenements (1..N),
- dump (target/probe),
- ping.


## D�tail de l'utilisation des propri�t�s cot� serveur
Voir la DEFINITION.md pour le d�tail, en particulier la fonction d'acces � la ressource depuis l'url configur�e par l'application.


*Et voila!*

