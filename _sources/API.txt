.. _coreapi:

========
Core API
========

Pool
====

The Pool class represents the entire caching system and all of the items in it. Objects of this class are used to
retrieve Items from the cache, as well as to perform bulk operations on the system such as Purging or Clearing the
cache.

clear
-----

*clear()*

The clear function completely empties all items associated with the Pool. After calling this every Item will be considered a miss and will have to be regenerated.


__construct
------------

*__construct(DriverInterface $driver = null)*

The constructor takes a Driver class which is used for persistent storage. If no driver is provided then the Ephemeral driver is used by default.


commit
------

*commit()*

The commit function persists any Item that was put into the Pool using the savedDeferred function and hasn't been saved yet.


deleteItems
-----------

*deleteItems(array $keys)*

Removes items from the Pool based off of their keys.


getDriver
---------

*getDriver()*

Returns the underlying driver used by the Pool to persist data.


getItem
-------

*getItem($key)*

The getItem function takes in a key and returns an associated Item object. The structure of keys can be found on the
:ref:`basics` page.


getItems
--------

*getItems(array $keys)*

The getItems function takes in an array of keys and returns an Iterator object populated by Item objects for
those keys. The structure of keys can be found on the :ref:`basics` page.


getNamespace
------------

*getNamespace()*

Retrieves the current namespace, or false if one isn't set.


hasItem
-------

*hasItem($key)*

Returns true if the specified key has a value stored in the Pool. A safer way to do this would be to use the isHit/isMiss
functions in the Item class.


purge
-----

*purge()*

The Purge function allows drivers to perform basic maintenance tasks, such as removing stale or expired items from
storage. Not all drivers need this, as many interact with systems that handle that automatically.

It's important that this function is not called from inside a normal request, as the maintenance tasks this allows can
occasionally take some time.


save
----

*save($item)*

Takes an Item class and persists it to the Pool.


saveDeferred
------------

*saveDeferred($item)*

Takes an Item class and eventually persists it to the Pool. This allows Stash to buffer cache operations and send them together,
optimizing the number of cache calls.


setDriver
---------

*setDriver($driver)*

Sets the driver for use by the caching system. This driver handles the direct interface with the caching backends,
keeping the system specific development abstracted out.


setItemClass
------------


setLogger
---------

*setLogger($logger)*

Sets a PSR LoggerInterface style logging client to enable the tracking of errors.


setNamespace
------------

*setNamespace($namespace)*

Places the Pool inside of a "namespace". All Items inside a specific namespace should be completely segmented from all
other Items.



Item
=====

The Item class represents specific pieces of data in the caching system. Item
objects are created by the Pool class.


clear
-----

*clear()*

The clear function removes the current Item's data from the backend storage.

If hierarchical or "stackable" caching is being used this function will also remove children Items. The Key section of
the :ref:`basics` document goes into more detail about how that works.


disable
-------

*disable()*

The disable function prevents any read or write operations and forces all the other calls to fail gracefully.


expiresAfter
------------

*expiresAfter(int|\DateInterval $time)*

Sets the expiration based off of an integer or DateInterval.


expiresAt
---------

*expiresAt(\DateTimeInterface $expiration)*

Sets the expiration to a specific time.


extend
------

*extend($ttl = null)*

This extends the Item's lifetime without changing it's data. Like the set function, the ttl can be a DateTime or
integer.


get
---

*get($invalidation)*

Retrieves the stored value of the Item or null if one is not set. Because null can be a valid stored object it is
important to call *isMiss* in order to actually check it's validity.


getCreation
-----------

*getCreation()*

This returns a DateTime of the Item's creation time, if it is available.


getExpiration
-------------

*getExpiration()*

This returns a DateTime of the Item's expiration time, if it is available.


getKey
------

*getKey()*

The getKey function returns this Item's key as a string. This is particularly useful when the Item is returned as a
group of Items in an Iterator.


isDisabled
----------

*isDisabled()*

Returns true if Stash has been disabled (typically via the disable function).


isHit
-----

*isHit()*

The isHit function returns false when the current Item has either stale or no data. Since Stash is capable of storing
both null and false values and returning them via the get function this is the only real way to test whether a cached
value is usable or not.

The exact behavior used to define a cache miss is defined by the invalidation method used for the object. The
:ref:`invalidation` page contains much more information about hos to use this functionality.



isMiss
------

*isMiss()*

This function is the opposite of isHit. Is is here for backwards compatibility, but to be PSR-6 compliant use the
isHit function instead.


lock
----

*lock($ttl = null)*

This should be called right before the script attempts to regenerate data from a cache miss. It signifies to other
processes or requests that the data is being generated and allows them to take special action to improve system
performance. Put more simply, just call this function and your cache will be higher performing as a result.

The exact effect of this function depends on which invalidation method is being used. The :ref:`invalidation` page
contains much more information about how to use this functionality.


save
----


set
---

*set($data)*

The set function is used to populate the cache with data. The only argument can be any type of data that is able to be
serialized- essentially everything except resources and classes which can't be serialized.



setInvalidationMethod
---------------------

*setInvalidationMethod($invalidation == Invalidation::PRECOMPUTE, [$args])*

This function can take a series of optional arguments defining how it handles cache misses. The first of these
options is the invalidation method to be used, while the other options all provide invalidation specific options. The
:ref:`invalidation` page contains much more information about hos to use this functionality.


setLogger
---------


setTTL
------

*setTTL($ttl = null)*

The second argument defines how long the Item will be stored in the cache. This is a maximum time, as Items can be
cleared or removed earlier but will never be considered a cache hit after it. This argument can either be a DateTime
defining a specific expiration or an integer representing the time, in seconds, that the data should be considered
fresh.





Drivers
=======

Stash works by storing values into various backend systems, like APC and Memcached, and retrieving them later. With the
exception their creation and setup, drivers don't have any "public" functions- they are used by the Pool and Item
classes themselves to interact with the underlying cache system.

The :ref:`Drivers` page contains a list of all drivers and their options.

setOptions
----------

*setOptions(array $options)*

Passes an array of options to the Driver. This can include things like server addresses or directories to use for cache
storage.


DriverList
==========

The DriverList class contains functions that are useful for people integrating or extending Stash. It primarily provides
information on what Drivers are available.

getAvailableDrivers
-------------------

*DriverList::getAvailableDrivers()*

Returns an associative array, $name => $class, of Drivers that can be enabled on this system.


getAllDrivers
-------------

*DriverList::getAllDrivers()*

Returns an associative array, $name => $class, of all Drivers regardless of whether they can run on this system.


getDriverClass
--------------

*DriverList::getDriverClass($name)*

Returns the class name of the requested driver.


registerDriver
--------------

*DriverList::registerDriver($name, $class)*

Adds a new Driver to the list of system drivers. This is used for extending Stash with custom drivers.