# apphooks

Application hooks is an attempt to define an API and protocol that will offer benefits in system configuration consistency and elements ressources access.

### Definition
An apphooks is a simple Ruby script that add/update/remove an element to an external application, and correctly handle failures/unreachable status to these applications.

From the server view, each script is represented as a queue. Only one call is made at a time to the remote application/service.

The script is able to set some elements properties on the server side.

The script accept three arguments:
* running mode
* element properties
* old element properties (for *update* only)

The script must implement three running modes:
* *add*:    to effectively register an element on a remote application,
* *update*: to update an allready added element on a remote application,
* *remove*: to remove an allready added element on a remote application.

To effectively set some element properties, the script must write a string to STDOUT.

Here is an example of a script started in "update" mode, receiving new and old properties,
and returning a valid string to set element property.
```sh
$ ./myapphook.rb update "prop1=myprop1;prop2=myprop2..." "prop1=myoldprop1;prop2=myoldprop2..."
SET;a_property=newprop;other_property=otherprop;apphook_ui_myapphook=http://myappaddress/elementId
$ echo $?
0
```

Another example of a valid script in "remove" mode, deleting some properties.
```sh
$ ./myapphook.rb remove "prop1=myprop1;apphook_ui_myapphokk=http..."
DELETE;apphook_ui_myapphook;another_prop
$ echo $?
0
```

The magic number *255* is the error return code

### Handling unreachable applications

- if apphooks:(*any*) return 0, the script is successfull
- if apphooks:(*any*) return an integer N from 1 to 254, the remote application is (for any reason) unreachable. The manager will run the script again in N*N minutes (ie: if return 2, will run the script in 4 minutes)
- if apphooks:(*any*) return 255 the apphooks has failed and do not have any way to recovery. From now, the hook is in failure state and will not accept any more commands (will have to be manualy debuged).
- if apphooks:*update* is pending (after an *update* positive integer return) and another apphooks:*update* is triggered, it is queued for later call.
- if apphooks:*update* is pending (after a positive integer return) and a apphooks:*remove* is triggered, the apphoooks:*update* is canceled.
- if apphooks:*add* is pending and apphooks:*update* is called, apphooks:*update* is queued for later call.
- if apphooks:*add* is pending and apphooks:*remove* is called, all actions are canceled (including potentialy pending updates).


### Element ressource access
Each apphooks can set or delete an element property when executed in all modes. These properties can be used by the manager UI to access the element ressources accross all external applications (ie: the URL to the element configuration), skipping external application specific welcome and list ressources pages.

### Usage example
- create an element on the manager,
- assign a apphook to the element: myhook will call myhook:add. Myhook set a property {"apphook_ui_myhook", "http://myhook-server/myElementId/"} to the created element, making it accessible via the manager main UI. The manger should recognize property begining by "apphook_ui" and apply his access method "myhook" if defined or use the value as an URI.
- modify some properties on the element (on the manager) will call myhook:update,
- delete the element (on the manager) will call myhook:remove.