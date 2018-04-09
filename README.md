# Plugins
List of SocketCluster plugins/modules.

If you've built a frontend or backend plugin for SocketCluster, feel free to make a PR and share it with the community.
The idea is that this repo will eventually turn into "a marketplace for SocketCluster plugins".

## Clients drivers

The curated list of client drivers for SC is here: https://github.com/SocketCluster/client-drivers

## Open source plugins

### Presence

- ```sc-stateless-presence``` https://github.com/SocketCluster/sc-stateless-presence - A simple client and server plugin which allow you to easily track user presence across channels in real-time. Works without the need for a datastore or database. Optimized for front end use.
- ```sc-presence``` https://github.com/coinigy/sc-presence - A presence plugin which uses MySQL to track user presence in real-time. Optimized for back end use.

### CRUD

**Frontend**

- ```sc-field``` https://github.com/SocketCluster/sc-field - A Polymer component which represents a realtime auto-updating data field from ```sc-crud-rethink```.
- ```sc-collection``` https://github.com/SocketCluster/sc-collection - A Polymer component which represents a realtime auto-updating collection from ```sc-crud-rethink```.
- ```vue-socketcluster``` https://github.com/happilymarrieddad/vue-socketcluster - A Vue component which integrates socketcluster-client directly into VueJS.

**Backend**

- ```sc-crud-rethink``` https://github.com/SocketCluster/sc-crud-rethink - A Create Read Update Delete (CRUD) module/layer to allow you to quickly write "server-less" realtime apps using SC and RethinkDB.
- ```sc-crud-mysql``` https://github.com/happilymarrieddad/sc-crud-mysql - A Create Read Update Delete (CRUD) module/layer to allow you to quickly write "server-less" realtime apps using SC and MySQL.
- ```sc-publish-out-queue``` https://github.com/happilymarrieddad/sc-publish-out-queue - Queue's up the outbound published messages so that the event loop doesn't get bottled up from thousands of messages being sent at one time.

### Protocol standardization

- ```wamp-socket-cluster``` https://github.com/LiskHQ/wamp-socket-cluster - A plugin which allows you to wrap SocketCluster to turn it into a compliant WAMP protocol server.

### Codecs

Codecs are useful for compressing entire SC messages into another intermediary format before they are are sent over the wire - They are also responsible for decompressing messages back into their original SC protocol format when they arrive on the other side.

- ```sc-codec-min-bin``` https://github.com/SocketCluster/sc-codec-min-bin - A codec for compressing SC messages into a minimal binary format before transmiting them over the wire. This is ideal for games and other high-throughput applications.
- ```sc-codec-pbf``` https://github.com/sammccord/sc-codec-pbf - A codec for decoding and encoding protocol buffers, a compact binary format for structured data serialization. Extremely fast and light, but requires that you only send objects that can be encoded with a proto message you define at configuration time, or strings.

### Broker engines

A broker engine is reponsible for sharing/synchronizing pub/sub channels (and optionally, data) between SC brokers. A broker engine is responsible for exposing an `exchange` object for interacting with message brokers on the backend. The API of different broker engines may vary.

- ```sc-redis-broker``` https://github.com/rapidops/sc-redis-broker - This broker engine allows your SC workers to synchronize their pub/sub channels directly through Redis (instead of going through sc-broker).


## Open source examples

- ```VueCluster``` https://github.com/VueCluster/VueCluster - A high-speed, server-less framework based off of Socketcluster and VueJS-2.

## Contribution guidelines

- Make sure your plugin has a Github repo and a README.md file.
- The name of your plugin (both frontend and backend) should start with ```sc-```
- Look at existing plugins above and try to follow conventions were possible.

**Frontend**

- Publish to bower.
- Other than that, it's whatever works; just keep it clean and documented!

**Backend**

- Publish to npm

Your backend plugin should expose an ```attach(scObject[, options, callback])``` method - The scObject argument should be an instance of one of the following
classes: SocketCluster, SCWorker, SCServer, SCSocket or Broker.
Right now, the user will have to attach your plugin manually to the appropriate scObject, but there are plans to automate this so that users will be able to
install your plugin using a ```socketcluster install mypluginname``` command.
The class of the scObject depends on which part of SC's architecture are affected:

If attached from master process ```server.js```, scObject will be:

- SocketCluster http://socketcluster.io/#!/docs/api-socketcluster


If attached from workerController ```worker.js```, scObject will be one of:

- SCWorker http://socketcluster.io/#!/docs/api-scworker
- SCServer http://socketcluster.io/#!/docs/api-scserver
- SCSocket http://socketcluster.io/#!/docs/api-scsocket-server

If attached from brokerController ```broker.js```, scObject will be:

- Broker http://socketcluster.io/#!/docs/api-broker

Internally, your plugin should extend the functionality of the scObject provided.

You should use private events instead of public ones (where possible) to prevent the user from interfering
with your plugin's logic.
For example, use the '_handshake', '_connection', '_disconnection' and '_disconnect' events
instead of their public counterparts 'handshake', 'connection', 'disconnection' and 'disconnect'...
