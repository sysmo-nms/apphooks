# apphooks

Application hooks is an attempt to define an API and/or protocol that will offer benefits in system configuration consistency and elements ressources access.

### Element configuration
An apphooks should be a simple script that add/update/remove an element to an external application, and correctly handle failures/unreachable status.

The script should be able to set some elements properties,

The script will accept 3 artuments:
* running mode
* element properties
* old element properties (for *update* only)

The script should accept three running modes:
* *add*:    to effectively register an element on a remote application,
* *update*: to update an allready added element on a remote application,
* *remove*: to remove an allready added element on a remote application.

Now the scripts rules:
- if apphooks:(*any*) return 0, the script is successfull
- if apphooks:(*any*) return a positive integer N, the remote application is (for any reason) unreachable. The manager will run the script again in N*N minutes (ie: if return 2, will run the script in 4 minutes)
- if apphooks:(*any*) return a negative integer, the apphooks has failed and do not have any way to recovery. From now, the hook is in failure state and will not accept any more commands (will have to be manualy debuged).
- if apphooks:*update* is pending (after an *update* positive integer return) and another apphooks:*update* is triggered, it is queued for later call.
- if apphooks:*update* is pending (after a positive integer return) and a apphooks:*remove* is triggered, the apphoooks:*update* is canceled.
- if apphooks:*add* is pending and apphooks:*update* is called, apphooks:*update* is queued for later call.
- if apphooks:*add* is pending and apphooks:*remove* is called, all actions are canceled (including potentialy pending updates).


### Element ressource access
Each apphooks can set or delete an element property when executed in all modes. These properties can be used by the manager UI to access the element ressources accross all external applications (ie: the URL to the element configuration), skipping external application specific welcome and list ressources pages.

### Usage example
- create an element on the manager,
- assign a apphook to the element (ie: confhook(simple) "register my conf from network") (will call confhook:add)
- assign a apphook to the element (ie: dochook(set property) "generate my shared documentation folder") (will call dochook:add). dochook set a property {"apphook_ui_dochook", "http://docapp.zozo/myElementId/"} to the created element, making it accessible via the manager main UI. The manger should recognize property begining by "apphook_ui" and apply his access method "dochook" if defined or use the value as an URI.
- modify some properties on the element (on the manager) (will call confhook:update and dochook:update)
- delete the element (on the manager) (will call confhook:remove and dochook:remove)