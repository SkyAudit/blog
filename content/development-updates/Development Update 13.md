+++
title = "Development Update #13"
tags = [
    "Development",
]
date = "2014-04-10"
categories = [
    "Development Update",
]
description = "Release notes highlighting the current development behind Skycoin  "
+++

## Summary:

The "services" architecture prototype is done.
Link: https://github.com/skycoin/skywire/blob/master/poolExample2.go

##### Raw Socket/Connection Pool:
https://github.com/skycoin/skywire/blob/master/poolExample0.go

##### Connection Pool with Dispatcher Helper and message serialization/deserialization:
https://github.com/skycoin/skywire/blob/master/poolExample1.go

##### Services Architecture Prototype:
https://github.com/skycoin/skywire/blob/master/poolExample2.go

Each code example is more complex/higher level than the previous one.

A channel is like a port and each application/service receives messages on a channel. Each service has an object holding state and a set of messages (golang structs) it can receive and send, which are registered at service initialization. When a message is received, the Handle() message is called on the message.  Service have an OnConnect and OnDisconnect callback.

Channel 0 is reserved for a special service that initiates connections between other service and offers service introspection. The service on channel 0 will receive a connection/disconnect for all connecting/disconnecting clients. The channel 0 service mediates connections between services on different hosts.

In this example there are two services, SkywireDaemon (channel 0) and TestServiceServer (channel 1). A connection is created between the connection pools and SkywireDaemon is used to send a ServiceConnectMessage. The remote peer responds to the message and a connection between the TestServiceServer instances is made. This triggers the OnConnect callback on TestServiceServer. Then a TestMessage message is sent via the TestServiceServer instance and the Handle() method is called on the message on the receiving end.

You can run the example with `go run poolExample2.go`.

Blockchain synchronization and transaction relay will be exposed as "services". The service daemon (channel 0) will have functionality for finding peers for services (DHT, PEX), maintaining peer lists and handling connection timeouts.

This is a framework for creating distributed, peer to peer applications on the Skycoin Wire protocol. The top level will create a service daemon, import the desired services and then associate them with the service manager. This architecture enables extensibility through third party applications. This also enables multiple service server instances to be instantiated in a single daemon process, enabling clients to sync multiple blockchains with a single daemon and connection pool (important for personal blockchains and Obelisk servers).
