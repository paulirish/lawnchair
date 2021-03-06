Adapters
========

Adapters expose a consistent interface to a persistent storage implementation. A Lawnchair build enqueues adapters and mixes in the first one valid for the current environment. This pattern is common in mobile scenarios, for example, a Lawnchair build with the DOM and Gears adapters will gracefully degrade through all available Android persistence solutions.

	blackberry-persistent-store ... great for phonegap
	dom ........................... default adapter
	gears-sqlite .................. for androids < 2.x
	ie-userdata ................... for msft js engines
	memory ........................ in memory reference implemenation
	webkit-sqlite ................. great for webkits
	window-name ................... oldest hack in the book

If you require an adapter thats not listed here it is trivial to implement your own. Adapters must have the following interface:

    adapter ........................ adapter name 
    valid .......................... true if the adapter is valid for the current environment
    init ([options], callback) ..... ctor call and callback. 'name' is the most common option (to name the collection) 
    keys (callback) ................ returns all the keys in the store
    save (obj, callback) ........... save an object
    batch(array, callback) ......... batch save an array of objects
    get (key|array, callback) ...... retrieve an object (or array of objects) and apply callback to each 
    exists (key, callback) ......... check if a document exists
    all (callback) ................. returns all the objects to the callback as an array
    remove (key|array, callback) ... remove a document or collection of documents
    nuke (callback) ................ destroy all documents

The tests ensure adapters are consistent no matter what the underlying store is. If you are writing an adapter check out `./tests/lawnchair-spec.js`. The memory adaptor is probably the simplest implementation to learn from. Note, all Lawnchair methods accept a callback as a last parameter. This is deliberate, most modern clientside storages only have async style interfaces, for a good reason, your code won't block the main thread aiding in the perception of performance. That callback will be scoped to the lawnchair instance. Make use of `fn` and `lambda` methods to allow for terse callbacks. 
