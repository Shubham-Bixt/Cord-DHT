# Chord-DHT
Implementation of Chord P2P Distributed Hash Table

More Information about Chord Protocol can be found here - https://en.wikipedia.org/wiki/Chord_(peer-to-peer)

Typing "help" will show all supported commands


## Implementation ##
Each node will be assigned a unique ID (within 2^m (m is 48 in this case)) by hashing key which will be "ip:port" of that node by SHA-1 Algorithm

A node can create a DHT ring, other nodes can join the ring by contacting this node using it's ip and port number

When a node creates or joins a ring, it spawns multiple threads which are responsible for tasks like listening to other nodes to join the
ring, sending acknowledgement and other operations , doing stabilization tasks to ensure correct finger table entries, successor 
and predecessor pointers and a Successor list.

Each node in the network is assigned a unique identifier by hashing its ip:port using the SHA-1 algorithm. The identifier space is bounded within 2^m, with m = 48 in this implementation—allowing up to 2^48 unique node IDs.

Nodes can either create a new DHT ring or join an existing one by connecting to another node's IP and port.

When a node joins or creates a ring, it starts several background threads. These threads handle:

  *  Communication with other nodes
    
  *  Stabilization procedures to update routing structures
    
  *  Maintaining links to immediate neighbors (successor and predecessor)
    
  *  Updating and managing the finger table (used for efficient lookups)
    
  *  Managing a successor list (used for fault tolerance)

Each node's finger table contains m = 48 entries. In addition, a successor list with r = 10 entries helps a node quickly recover if its successor leaves the network.

Nodes periodically ping their successors and predecessors to check connectivity. If a neighbor is unresponsive, stabilization logic ensures the node reconnects appropriately within the ring.

For data storage, each node maintains a key-value store. When a key-value pair is inserted:

The key is hashed.

The data is routed to and stored on the appropriate node whose ID is equal to or immediately follows the key's hash.

When a new node joins the ring, it takes ownership of keys that now fall under its responsibility by requesting those from its successor. Conversely, if a node leaves, it transfers its keys to its successor before exiting the ring.



## File Structure ##

* main.cpp: Entry point of the application

* functions.cpp: Contains core operational logic

* NodeInformation class: Encapsulates a node’s attributes and the core functionality needed to participate in and maintain the DHT ring

* SocketAndPort class: Manages socket creation and connection info; used within NodeInformation

* HelperClass: Provides utility functions to assist with hashing, comparisons, and communication


## Supported Commands ##

typing "help" in the terminal shows all supported commands

__create__ - will create a DHT ring

__join "ip" "port"__ - will connect to the main node running at IP address <ip> and Port Number <port>

__printstate__ - will print successor, predecessor, fingerTable and Successor list of that node

__print__ - will print all keys and values present in that node

__port__ - will display port number on which node is listening

__port "number"__ - will change port number to mentioned number if that port is free (will only run before the node has joined the ring)

__put "key" "value"__ - will put key and value to the node it belongs to

__get "key"__ - will get value of mentioned key


## Execution ##

A Makefile has also been included. Just type make to build the whole project
