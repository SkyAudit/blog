+++
title = "Development Update #38"
tags = [
    "Development",
]
date = "2014-09-23"
categories = [
    "Development Updates",
]
description = "Release notes highlighting the current development behind Skycoin."
+++

## Summary:

Two separate people are independently working on the distribution script. Should get done soon.

Another dev is trying to fix the merge conflict and error that is appearing on the github repo.

Documentation and descriptions of software libraries that need to be written, will be placed on Github immediately. We are not waiting for Aether/Merkle-DAG to be completed to release documents. Document releases will be smaller, more frequent.

Tutorials for developers are going on a github wiki.

Initially we thought, the darknet would be working fairly quickly and we could run the whole client on top of it. However, now parts of the coin could not be finished because it required infrastructure that was not implemented yet. We are moving away from relying upon infrastructure that is not completed and focusing on what we can get done now

##### In particular
- The darknet will be in Golang, not C
- The darknet will be separate from the daemon for the coin, until its ready
- The coin will use existing modules and will not transition to Merkle-DAG for block storage, until Merkle-DAG is working

We will release a prototype consensus algorithm and run the chain on top of that, with some safe guards (developer signing key) to deal with attacks/errors that arise. This will protect against loss of coins, while allowing us to launch. The consensus mechanism will be deployed in stages, instead of trying to make it perfect and 100% implemented at launch (which may delay launch another year).
