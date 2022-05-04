
# Associations
OpenBMC uses associations to extend the DBUS API without impacting existing objects and interfaces.
## `org.openbmc.Associations`
A object wishing to create an association implements this interface.

methods:
* None

properties:
* `associations signature=a(sss):` An array of forward, reverse, endpoint tuples where:
 * fowrard - the type of the association to create
 * reverse - the type of the association to create for the endpoint
 * endpoint - the endpoint of the association

For example, given an object: `/org/openbmc/events/1`
that implements `org.openbmc.Associations` and sets the `associations` property to:

`associations: [`

`    ["events", "frus", "/org/openbmc/piece_of_hardware"],`

`    ["events", "times", "/org/openbmc/timestamps/1"]`

`]`

would result in the following associations:

* /org/openbmc/events/1/frus
* /org/openbmc/events/1/times
* /org/openbmc/piece_of_hardware/events
* /org/openbmc/timestamps/1/events

It is up to the specific OpenBMC implementation to decide, what, if anything to do with these.
For example, the Phosphor mapper application looks for objects that implement this interface
and creates objects in its /org/openbmc DBUS namespace.

## `org.openbmc.Association`
OpenBMC implementations use this interface to insert associations into the DBUS namespace.

methods:
* None

properties:
* `endpoints signature=as:` An array of association endpoints.  
		   
For example, given:

/org/openbmc/events/1/frus:

`endpoints: [`

`    ["/org/openbmc/hardware/cpu0"],`

`    ["/org/openbmc/hardware/cpu1"],`

`]`
   
Denotes the following:
* /org/openbmc/events/1 => fru => /org/openbmc/hardware/cpu0
* /org/openbmc/events/1 => fru => /org/openbmc/hardware/cpu1
-- 
