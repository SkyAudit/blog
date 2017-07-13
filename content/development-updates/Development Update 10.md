+++
title = "Development Update #10"
tags = [
    "Development",
    "Skywire",
]
date = "2014-03-23"
categories = [
    "Development Updates",
]
description = "Release notes highlighting the current development behind Skycoin  "
+++

## Summary:
The new wire protocol is more work than expected. Instead of delaying launch by two weeks, to address these issues I am hacking together something that will get the network running.

##### Multiple software components need to be be refactored:
- Daemon needs to be extended to support multiple connection pools with independent peer exchange and using a common listening socket
- gnet needs to be modified to support an introduction packet
- gnet packet handling callbacks need to be embedded in the gnet object instead of globals, to allow multiple gnet instances in the same application
- daemon DHT functionality for peer bootstrap needs to be abstracted to allow for retrieving bootstrap peers for multiple hashes.
- The hashchain replicator wire protocol needs to be designed and implemented
- The blob and hashchain replicators need to be abstracted on top of the new connection pool

## Announcement:

The new wire protocol is now in its own repo as a library. github: https://github.com/skycoin/sync

run.go is a simple example of spawning two instances and replicating blob data between the instances. Writing tech demo for synchronization of a cryptographically authenticated blob data and running a webserver off the synchronized data.
