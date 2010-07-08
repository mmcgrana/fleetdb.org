---
layout: page
title: "Docs: Query Reference : info"
docs_tab: "qr"
---

info
-------

    ["info"]
    => {"fleetdb-version": <version>
        "persistent":      false}

    ["info"]
    => {"fleetdb-version": <version>
        "persistent":      true
        "db-file-size":    <size>
        "compacting":      <true|false>}
    
    

Returns metadata about the running FleetDB instance. This metadata always includes a string indicating the version of the FleetDB software and a boolean indicating whether the database is running in ephemeral or persistent mode. If the database is running in persistent mode, the size of the database file in bytes and a boolean indicating whether the database file is currently being compacted is also included.
