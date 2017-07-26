# Node related requests

Nodes are the entities that exchange data in a network. A node is essentially a NIS instance running on a computer. To be able to communicate with the network, a node needs to be booted. Through node requests it is possible to discover other nodes in the network, learn about other nodes experiences and get information about their current chain height.

[Official Source](https://bob.nem.ninja/docs/#node-related-requests)

# NodeHttp definition

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

# NodeHttp usage

```typescript
import {NodeHttp, NEMLibrary, NetworkTypes} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const nodeHttp = new NodeHttp({domain: "104.128.226.60"});
nodeHttp.getNodeInfo().subscribe(node => {
    console.log(node);
});
```

Output:

```
Node {
  metaData: 
   NodeMetaData {
     features: 3,
     network: -104,
     application: null,
     version: '0.6.92-BETA',
     platform: 'Oracle Corporation (1.8.0_40) on Linux' },
  endpoint: NodeEndpoint { protocol: 'http', port: 7890, host: '104.128.226.60' },
  identity: NodeIdentity { name: 'Hi, I am BigAlice2', publicAccount: undefined } }
```

[Run the code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/concepts/node/NodeHttpExample.ts)

# Models

## Node

```typescript
/**
 * Nodes are the entities that perform communication in the network like sending and receiving data.
 * A node has an identity which is tied to an account through which the node can identify itself to the network.
 * The communication is done through the endpoint of the node. Additionally a node provides meta data information.
 */
export declare class Node {
    
    /**
     * Denotes the beginning of the meta data substructure.
     */
    readonly metaData: NodeMetaData;
    
    /**
     * Denotes the beginning of the endpoint substructure.
     */
    readonly endpoint: NodeEndpoint;
    
    /**
     * Denotes the beginning of the identity substructure.
     */
    readonly identity: NodeIdentity;
    
}

/**
 * Node meta data
 */
export declare class NodeMetaData {
    
    /**
     * The number of features the nodes has.
     */
    readonly features: number;
    
    /**
     * The network id
     */
    readonly network: NetworkTypes;
    
    /**
     * The name of the application that is running the node.
     */
    readonly application: string;
    
    /**
     * The version of the application.
     */
    readonly version: string;
    
    /**
     * The underlying platform (OS, java version).
     */
    readonly platform: string;
}

/**
 * Node endpoint
 */
export declare class NodeEndpoint {
    
    /**
     * The protocol used for the communication (HTTP or HTTPS).
     */
    readonly protocol: string;
    
    /**
     * The port used for the communication.
     */
    readonly port: number;
    
    /**
     * The IP address of the endpoint.
     */
    readonly host: string;
}

/**
 * Node identity
 */
export declare class NodeIdentity {
    
    /**
     * The name of the node.
     */
    readonly name: string;
    
    /**
    * The public account used to identify the node.
    */
    readonly publicAccount: PublicAccount;
}

```

## NisNodeInfo

```typescript
/**
 * A NodeCollection object holds arrays of nodes with different statuses.
 */
export declare class NisNodeInfo {
    
    /**
     * Denotes the beginning of the node substructure.
     */
    readonly node: Node;
    
    /**
     * Denotes the beginning of the application meta data substructure.
     */
    readonly nisInfo: ApplicationMetaData;
}

/**
 * The application meta data object supplies additional information about the application running on a node.
 */
export declare class ApplicationMetaData {
    
    /**
     * The current network time, i.e. the number of seconds that have elapsed since the creation of the nemesis block.
     */
    readonly currentTime: number;
    
    /**
     * The name of the application running on the node.
     */
    readonly application: string;
    
    /**
     * The network time when the application was started.
     */
    readonly startTime: number;
    
    /**
     * The application version.
     */
    readonly version: string;
    
    /**
     * The signer of the certificate used by the application.
     */
    readonly signer: string;
}
```

## NodeCollection

```typescript
/**
 * A NodeCollection object holds arrays of nodes with different statuses.
 */
export declare class NodeCollection {
    
    /**
     * A connection to the node cannot be established.
     */
    readonly inactive: Node[];
    
    /**
     * A connection can be established and the remote node responds in a timely manner.
     */
    readonly active: Node[];
    
    /**
     * A connection can be established but the node cannot provide information within the timeout limits.
     */
    readonly busy: Node[];
    
    /**
     * A fatal error occurs when trying to establish a connection or the node couldn't authenticate itself correctly.
     */
    readonly failure: Node[];
}
```

## ExtendedNodeExperience

```typescript
/**
 * When exchanging data with other nodes the result of the communication is divided into three
 * different outcomes: success, neutral and failure.
 * In the cases of success and failure the result is saved to be able to judge the quality of a node.
 * This has influence on the probability that a certain node is selected as partner.
 */
export declare class ExtendedNodeExperience {
    
    /**
     * Denotes the beginning of the of the Node substructure.
     */
    readonly node: Node;
    
    /**
     * The number of synchronization attempts the node had with the remote node.
     */
    readonly syncs: number;
    
    /**
     * Denotes the beginning of the of the NodeExperience substructure.
     */
    readonly experience: ExtendedNodeExperienceData;
}

/**
 * Node experience data
 */
export declare class ExtendedNodeExperienceData {
    
    /**
     * The number of successful communications with the remote node.
     */
    readonly s: number;
    
    /**
     * The number of failed communications with the remote node.
     */
    readonly f: number;
}

```
