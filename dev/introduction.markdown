---
layout: page
dev_tab: true
---

The [FleetDB source](http://github.com/mmcgrana/fleetdb) and [issue tracker](http://github.com/mmcgrana/fleetdb/issues) are hosted on GitHub.

To get started with developing FleetDB, first ensure that you have [Git](http://git-scm.com/) and [Leiningen](http://github.com/technomancy/leiningen) installed. Then clone the FleetDB repository, pull in the dependencies, compile the project, and run the tests as follows:

    $ git clone git://github.com/mmcgrana/fleetdb.git
    $ lein deps
    $ lein compile-java
    $ clj test/run.clj --no-server

The above assumes a `clj` script that adds the `src` and `test` directories and the `lib/*.jar` files to your classpath. Your local setup may be different.

To fully test the server daemon as a standalone process, execute the following in three separate shells:

    $ java -cp fleetdb-standalone.jar fleetdb.server -e -p 3400
    $ java -cp fleetdb-standalone.jar fleetdb.server -e -p 3401 -x pass
    $ clj test/run.clj
