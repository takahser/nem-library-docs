### How to get NEM Node information

```typescript
import {NEMLibrary, NetworkTypes, NodeHttp} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const nodeHttp = new NodeHttp({domain: "104.128.226.60"});
nodeHttp.getNodeInfo().subscribe(node => console.log(node));

```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/node/How_to_get_NEM_Node_information.ts)


### How to get extended information about a NEM Node

```typescript
import {NEMLibrary, NetworkTypes, NodeHttp} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const nodeHttp = new NodeHttp({domain: "104.128.226.60"});
nodeHttp.getNisNodeInfo().subscribe(nisNodeInfo => console.log(nisNodeInfo));
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/node/How_to_get_extended_information_about_a_NEM_Node.ts)

### How to get all active nodes

```typescript
import {NEMLibrary, NetworkTypes, NodeHttp} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const nodeHttp = new NodeHttp({domain: "104.128.226.60"});
nodeHttp.getActiveNodes().subscribe(nodes => console.log(nodes));
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/node/How_to_get_all_active_nodes.ts)
