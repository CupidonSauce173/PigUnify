# PigUnify

## Introduction

PigUnify will be a C# server that will unify all Pig plugins together for better performance and the uses of less threads in a network. The goal is to create a TCP server in a C# environment so all plugins can connect to it.

## Goals

The PigUnify server will create the same objects as the plugins and store them in the memory. The exact method isn't yet decided but it will probably be stored in a Redis server.
The server will send in a json type the data to the clients (plugins) via a TCP server. 

As it is now, the plugins gather data from the MySQL databases and then create objects (e.g. Request, Friend, Notification, etc) over threads, but here's the problem, each plugin creates one or multiple threads. If using each of the listed plugins, you will have (excluding PMMP threads) 5 extra threads, now, if you have multiple instances of a PMMP server running with those plugins, you will have even more threads running (if having 5 PMMP instances, you will have 25 extra threads just with 3 plugins). So the goal here is to reduce the amount of threads and also make them do less heavy tasks such as querying & processing data. Instead, you would have only one thread per plugin used to decode json types, sending packets to PigUnify and receiving them.

## Plugins to support

- PigNotify
- PigFriends
- PigTranslate

### Notes

For PigTranslate, I want to create an array of LibreTranslate instances, PigUnify will take care of doing load balancing and redirect requests from the clients to unused LibreTranslate instances.
