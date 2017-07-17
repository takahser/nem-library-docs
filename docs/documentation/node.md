# Node related requests

Nodes are the entities that exchange data in a network. A node is essentially a NIS instance running on a computer. To be able to communicate with the network, a node needs to be booted. Through node requests it is possible to discover other nodes in the network, learn about other nodes experiences and get information about their current chain height.

[Official Source](https://bob.nem.ninja/docs/#node-related-requests)

## NodeHttp definition

```typescript
export declare class NodeHttp extends HttpEndpoint {
    constructor(serverConfig?: ServerConfig);

    /**
     * Gets basic information about a node
     * @returns Observable<Node>
     */
    getNodeInfo(): Observable<Node>;

    /**
     * Gets extended information about a node
     * @returns Observable<NisNodeInfo>
     */
    getNisNodeInfo(): Observable<NisNodeInfo>;

    /**
     * Gets an array of all known nodes in the neighborhood.
     * @returns Observable<NodeCollection>
     */
    getAllNodes(): Observable<NodeCollection>;

    /**
     * Gets an array of all nodes with status 'active' in the neighborhood.
     * @returns Observable<Node[]>
     */
    getActiveNodes(): Observable<Node[]>;

    /**
     * Gets an array of active nodes in the neighborhood that are selected for broadcasts.
     * @returns Observable<Node[]>
     */
    getActiveNeighbourNodes(): Observable<Node[]>;

    /**
     * Requests the chain height from every node in the active node list and returns the maximum height seen.
     * @returns Observable<BlockHeight>
     */
    getMaximumChainHeightInActiveNeighborhood(): Observable<BlockHeight>;

    /**
     * Requests the chain height from every node in the active node list and returns the maximum height seen.
     * @returns Observable<ExtendedNodeExperience[]>
     */
    getNodeExperiences(): Observable<ExtendedNodeExperience[]>;
}
```

## NodeHttp usage

```typescript
import {NodeHttp, NEMLibrary, NetworkTypes} from "nem-library";

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const nodeHttp = new NodeHttp({domain: "104.128.226.60"});
nodeHttp.getNodeInfo().subscribe(node => {
    console.log(node);
});
```

