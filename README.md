# plugins
List of SocketCluster plugins/modules.

If you've built a frontend or backend plugin for SocketCluster, feel free to make a PR and share it with the community.

### Backend

- sc-crud-rethink https://github.com/SocketCluster/sc-crud-rethink

### Frontend

- sc-field https://github.com/SocketCluster/sc-field
- sc-collection https://github.com/SocketCluster/sc-collection

## Contribution guidelines

- Make sure you have a README.md file.
- The name of your plugin (both frontend and backend) should start with ```sc-```
- Look at existing plugins above and try to follow conventions were possible.

**Frontend**

- Publish to bower.
- Other than that, it's whatever works; just keep it clean and documented!

**Backend**

- Publish to npm

Your backend plugin should expose an ```attach(scObject[, options, callback])``` method.
The scObject which is provided to your plugin should be one of:

If attached from ```server.js```:

- SocketCluster http://socketcluster.io/#!/docs/api-socketcluster


If attached from ```worker.js```:

- SCWorker http://socketcluster.io/#!/docs/api-scworker
- SCServer http://socketcluster.io/#!/docs/api-scserver
- SCSocket http://socketcluster.io/#!/docs/api-scsocket-server

If attached from ```broker.js```:

- Broker http://socketcluster.io/#!/docs/api-broker

Your plugin can then use the scObject provided to extend the functionality of SocketCluster.

You should use private events instead of public ones (where possible) to prevent the user from interfering
with your plugin's logic.
For example, use the '_handshake', '_connection', '_disconnection' and '_disconnect' events
instead of their public counterparts 'handshake', 'connection', 'disconnection' and 'disconnect'...
