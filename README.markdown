Wasabi Redis
============

Wasabi Redis is a Scala-centric Redis API. It wraps a (somewhat) type-safe veil around [Jedis](https://github.com/xetorthio/jedis), while attempting to make the result look more like Scala and less like the Redis wire protocol.

Redis has what's essentially a dynamic type system, consisting of _ordinals_ (integers), _byte arrays_ (strings), _lists_, _hashes_, _sets_ and _sorted sets_. Wasabi expresses Redis cache entries as *RValue*, of which there are:

*   RString
*   RLong
*   RHash
*   RList
*   RSet
*   RSortedSet

Each has a Scala-looking API.

Installation
------------
At the moment, old-school: 

1. have SBT 0.10.1+
2. clone this repo
3. go **sbt publish_local**

Usage
-----

Point an instance of Wasabi at your Redis:

    import org.wasabiredis._
    
    val wasabi = new Wasabi()
    val detailOrientedWasabi = new Wasabi("localhost", 6973)

And ask it for the _RValue_ at the key:

    val rl = wasabi.long("some_counter")
    val rs = wasabi.string("I've seen the future!")
    val rh = wasabi.hash("map_of_strings")
    val rl = wasabi.list("shopping")
    val rs = wasabi.set("disheveled_set")
    val ss = wasabi.sortedSet("sheveled_set")

We can't (yet?) interrogate Redis about the type of value at a given key, other than _try-and-hope-it-don't blow_.
It's up to you to make sure that types match what you expect.
You'll know by the stack traces when you miss.

All RValue APIs fall straight through to the underlying Jedis calls (which in turn fall straight through to Redis with minor marshalling).
There's no memoization going on.
What you see is what Redis sees.

RString
-------
    val s: RString = wasabi.string("long_story")
    s.get                       //  None
    
    s := "The quick brown fox still jumps over the lazy dog"
    s.get                       // Some("The quick ...")
    
    s += ", and has a chicken while at it"
    s.get                       // Some("The quick ... and has a chicken ...")

RLong
-----
    val l: RLong = wasabi.long("geiger_counter")
    l.get                       // None
    
    l := 1
    l.get                       // Some(1)
    
    l++                     
    l.get                       // Some(2)
    
    l += 42                     
    l.get                       // Some(43)
    
    l--                         
    l.get                       // Some(42)
    
    l -= 16                      
    l.get                       // Some(26)

RHash
-----
see source (for now)

RList
-----
**not implemented  yet**

RSet
----
see source (for now)

RSortedSet
----------
see source (for now)
