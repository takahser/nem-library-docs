# Mosaic related requests

NEM mosaics are assets that expose additional properties and other features. Each mosaic has an underlying mosaic definition. To be able to create a mosaic definition, an account must rent at least one root namespace which the mosaic definition can then refer to.

[Official Source](https://bob.nem.ninja/docs/#mosaics)

# MosaicHttp definition

```typescript
export declare class MosaicHttp extends HttpEndpoint {
    constructor(serverConfig?: ServerConfig);
    
    /**
     * Gets the mosaic definitions for a given namespace. The request supports paging.
     * @param namespace
     * @param id - The topmost mosaic definition database id up to which root mosaic definitions are returned. The parameter is optional. If not supplied the most recent mosaic definitiona are returned.
     * @param pageSize - The number of mosaic definition objects to be returned for each request. The parameter is optional. The default value is 25, the minimum value is 5 and hte maximum value is 100.
     * @returns Observable<MosaicDefinition[]>
     */
    getAllMosaicsGivenNamespace(namespace: string, id?: number, pageSize?: number): Observable<MosaicDefinition[]>;
    
    /**
     * Return the Mosaic Definition given a namespace and mosaic. Throw exception if no mosaic is found
     * @param {string} namespace
     * @param {string} mosaic
     * @returns {Observable<MosaicDefinition>}
     */
    getMosaicDefinition(namespace: string, mosaic: string): Observable<MosaicDefinition>;
    
    /**
     * Return a MosaicTransferable with the amount
     * @param {string} namespace
     * @param {string} mosaic
     * @param {number} amount
     * @returns {Observable<MosaicTransferable>}
     */
    getMosaicTransferableWithAmount(namespace: string, mosaic: string, amount: number): Observable<MosaicTransferable>;
}

```

# MosaicHttp usage

```typescript
import {MosaicHttp, NEMLibrary, NetworkTypes} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const mosaicHttp = new MosaicHttp({domain: "104.128.226.60"});
const namespace = "server";

mosaicHttp.getAllMosaicsGivenNamespace(namespace).subscribe(mosaicDefinitions => {
    console.log(mosaicDefinitions);
});
```

Output

```
[ MosaicDefinition {
    creator:
     PublicAccount {
       address: [Object],
       publicKey: '0e4573c386c5f891d2e61bfb5a96144fbd9881980b885751dba471ae1807dc34' },
    id: MosaicId { namespaceId: 'server', name: 'testmosaic' },
    description: 'descritpio',
    properties:
     MosaicProperties {
       initialSupply: 1000000,
       supplyMutable: true,
       transferable: true,
       divisibility: 0 },
    levy: undefined,
    metaId: 447 },
  MosaicDefinition {
    creator:
     PublicAccount {
       address: [Object],
       publicKey: '0e4573c386c5f891d2e61bfb5a96144fbd9881980b885751dba471ae1807dc34' },
    id: MosaicId { namespaceId: 'server', name: 'alcapone' },
    description: 'the one and only al capone',
    properties:
     MosaicProperties {
       initialSupply: 10000000,
       supplyMutable: true,
       transferable: true,
       divisibility: 0 },
    levy: MosaicLevy { type: 1, recipient: [Object], mosaicId: [Object], fee: 5 },
    metaId: 385 },
  MosaicDefinition {
    creator:
     PublicAccount {
       address: [Object],
       publicKey: '0e4573c386c5f891d2e61bfb5a96144fbd9881980b885751dba471ae1807dc34' },
    id: MosaicId { namespaceId: 'server', name: 'masteroftheworld' },
    description: 'description',
    properties:
     MosaicProperties {
       initialSupply: 100000000,
       supplyMutable: true,
       transferable: true,
       divisibility: 0 },
    levy: MosaicLevy { type: 1, recipient: [Object], mosaicId: [Object], fee: 5 },
    metaId: 384 } ]
```

[Run the code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/concepts/mosaic/MosaicHttpExample.ts)

# Models

## MosaicId

```typescript
/**
 * A mosaic id uniquely identifies an underlying mosaic definition.
 */
export declare class MosaicId {
    
    /**
     * The corresponding namespace id
     */
    readonly namespaceId: string;
    
    /**
     * The name of the mosaic definition.
     */
    readonly name: string;
    
    /**
     * constructor
     * @param namespaceId
     * @param name
     */
    constructor(namespaceId: string, name: string);
}

```

## Mosaic

```typescript
/**
 * A mosaic describes an instance of a mosaic definition. Mosaics can be transferred by means of a transfer transaction.
 */
export declare class Mosaic {
    
    /**
     * The mosaic id
     */
    readonly mosaicId: MosaicId;
    
    /**
     * The mosaic quantity. The quantity is always given in smallest units for the mosaic, i.e. if it has a divisibility of 3 the quantity is given in millis.
     */
    readonly quantity: number;
    
    /**
     * constructor
     * @param mosaicId
     * @param quantity
     */
    constructor(mosaicId: MosaicId, quantity: number);
}

```

## MosaicDefinition

```typescript

/**
 * A mosaic definition describes an asset class. Some fields are mandatory while others are optional.
 * The properties of a mosaic definition always have a default value and only need to be supplied if they differ from the default value.
 */
export declare class MosaicDefinition {
    
    /**
     * 	The public key of the mosaic definition creator.
     */
    readonly creator: PublicAccount;
    
    /**
     * The mosaic id
     */
    readonly id: MosaicId;
    
    /**
     * The mosaic description. The description may have a length of up to 512 characters and cannot be empty.
     */
    readonly description: string;
    
    /**
    * Mosaic properties 
    */
    readonly properties: MosaicProperties;
    
    /**
     * The optional levy for the mosaic. A creator can demand that each mosaic transfer induces an additional fee
     */
    readonly levy?: MosaicLevy;
    
    /**
     * The id for the mosaic definition object.
     */
    readonly metaId?: number;
    
    /**
     * constructor
     * @param creator
     * @param id
     * @param description
     * @param properties
     * @param levy
     * @param metaId
     */
    constructor(creator: PublicAccount, id: MosaicId, description: string, properties: MosaicProperties, levy?: MosaicLevy, metaId?: number);
}

/**
 * Each mosaic definition comes with a set of properties.
 * Each property has a default value which will be applied in case it was not specified.
 * Future release may add additional properties to the set of available properties
 */
export declare class MosaicProperties {
    
    /**
     * initialSupply: The creator can specify an initial supply of mosaics when creating the definition.
     * The supply is given in entire units of the mosaic, not in smallest sub-units.
     * The initial supply must be in the range of 0 and 9,000,000,000. The default value is "1000".
     */
    readonly initialSupply: number;
    
    /**
     * The creator can choose between a definition that allows a mosaic supply change at a later point or an immutable supply.
     * Allowed values for the property are "true" and "false". The default value is "false".
     */
    readonly supplyMutable: boolean;
    
    /**
     * The creator can choose if the mosaic definition should allow for transfers of the mosaic among accounts other than the creator.
     * If the property 'transferable' is set to "false", only transfer transactions having the creator as sender or as recipient can transfer mosaics of that type.
     * If set to "true" the mosaics can be transferred to and from arbitrary accounts.
     * Allowed values for the property are thus "true" and "false". The default value is "true".
     */
    readonly transferable: boolean;
    
    /**
     * The divisibility determines up to what decimal place the mosaic can be divided into.
     * Thus a divisibility of 3 means that a mosaic can be divided into smallest parts of 0.001 mosaics, i.e. milli mosaics is the smallest sub-unit.
     * When transferring mosaics via a transfer transaction the quantity transferred is given in multiples of those smallest parts.
     * The divisibility must be in the range of 0 and 6. The default value is "0".
     */
    readonly divisibility: number;
    
    /**
     * constructor
     * @param divisibility
     * @param initialSupply
     * @param supplyMutable
     * @param transferable
     */
    constructor(divisibility?: number, initialSupply?: number, transferable?: boolean, supplyMutable?: boolean);
}
```

## MosaicLevy

```typescript

export declare enum MosaicLevyType {
    Absolute = 1,
    Percentil = 2,
}

/**
 * A mosaic definition can optionally specify a levy for transferring those mosaics. This might be needed by legal entities needing to collect some taxes for transfers.
 */
export declare class MosaicLevy {
    
    /**
     * 	The levy type
     */
    readonly type: MosaicLevyType;
    
    /**
     * The recipient of the levy.
     */
    readonly recipient: Address;
    
    /**
     * The mosaic in which the levy is paid.
     */
    readonly mosaicId: MosaicId;
    
    /**
     * The fee. The interpretation is dependent on the type of the levy
     */
    readonly fee: number;
    
    /**
     * constructor
     * @param type
     * @param recipient
     * @param mosaicId
     * @param fee
     */
    constructor(type: MosaicLevyType, recipient: Address, mosaicId: MosaicId, fee: number);
}
```

## MosaicTransferable

```typescript

/**
 * Mosaic transferable model
 */
export declare class MosaicTransferable {
    
    /**
     * Quantity to be send
     */
    readonly quantity: number;
    
    /**
     * Mosaic definition properties
     */
    readonly properties: MosaicProperties;
    
    /**
     * Mosaic id
     */
    readonly mosaicId: MosaicId;
    
    /**
     * constructor
     * @param mosaicId
     * @param properties
     * @param quantity
     */
    constructor(mosaicId: MosaicId, properties: MosaicProperties, quantity: number);
    
    /**
     * Create a MosaicTransferable object with mosaic definition
     * @param mosaicDefinition
     * @param quantity
     * @returns {MosaicTransferable}
     */
    static createWithMosaicDefinition(mosaicDefinition: MosaicDefinition, quantity: number): MosaicTransferable;
}
```

## XEM

```typescript
/**
 * XEM mosaic transferable
 */
export declare class XEM extends MosaicTransferable {
   /**
    * constructor
    * @param amount
    */
    constructor(amount: number) {
        super(new MosaicId("nem", "xem"), new MosaicProperties(6, 8999999999, true, false), amount);
    }
}
```
