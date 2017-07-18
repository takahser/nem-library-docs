### How to get NEM Node information

```typescript
import {NEMLibrary, NetworkTypes, NodeHttp} from "nem-library";

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const nodeHttp = new NodeHttp({domain: "104.128.226.60"});
nodeHttp.getNodeInfo().subscribe(node => console.log(node));

```

### How to get extended information about a NEM Node

```typescript
import {NEMLibrary, NetworkTypes, NodeHttp} from "nem-library";

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const nodeHttp = new NodeHttp({domain: "104.128.226.60"});
nodeHttp.getNisNodeInfo().subscribe(nisNodeInfo => console.log(nisNodeInfo));
```

### How to get all active nodes

```typescript
import {NEMLibrary, NetworkTypes, NodeHttp} from "nem-library";

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const nodeHttp = new NodeHttp({domain: "104.128.226.60"});
nodeHttp.getActiveNodes().subscribe(nodes => console.log(nodes));
```