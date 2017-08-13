# Listeners


# AccountListener definition

```typescript
/**
 * Account listener
 */
export declare class AccountListener extends Listener {
    
    /**
     * Constructor
     * @param nodes
     */
    constructor(nodes?: WebSocketConfig[]);
    
    /**
     * Start listening updates
     * @param address
     * @returns {Observable<AccountInfoWithMetaData>}
     */
    given(address: Address): Observable<AccountInfoWithMetaData>;
}


```

# AccountListener usage

```typescript
/**
 * nem-library 0.3.0
 */

import {AccountListener, Address, NEMLibrary, NetworkTypes} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const address = new Address("TCJZJH-AV63RE-2JSKN2-7DFIHZ-RXIHAI-736WXE-OJGA");
let accountListener = new AccountListener().given(address).subscribe(x => {
    console.log(x);
}, err => {
    console.log(err);
});


```

Output

```
AccountInfoWithMetaData {
  balance: 
   Balance {
     balance: 10246300001,
     vestedBalance: 5507591402,
     unvestedBalance: 4738708599 },
  importance: 0,
  publicAccount: 
   PublicAccount {
     address: 
      Address {
        value: 'TCJZJHAV63RE2JSKN27DFIHZRXIHAI736WXEOJGA',
        networkType: 152 },
     publicKey: 'a4f9d42cf8e1f7c6c3216ede81896c4fa9f49071ee4aee2a4843e2711899b23a' },
  harvestedBlocks: 0,
  cosignatoriesCount: undefined,
  minCosignatories: undefined,
  status: 'LOCKED',
  remoteStatus: 'ACTIVE',
  cosignatoryOf: 
   [ AccountInfo {
       balance: [Object],
       importance: 0,
       publicAccount: [Object],
       harvestedBlocks: 0,
       cosignatoriesCount: undefined,
       minCosignatories: undefined },
     AccountInfo {
       balance: [Object],
       importance: 0,
       publicAccount: [Object],
       harvestedBlocks: 0,
       cosignatoriesCount: undefined,
       minCosignatories: undefined } ],
  cosignatories: [] }

```

[Run the code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/concepts/wallet/SimpleWalletExample.ts)


# BlockchainListener definition

```typescript
/**
 * Blockchain listener
 */
export declare class BlockchainListener extends Listener {
    
    /**
     * Constructor
     * @param nodes
     */
    constructor(nodes?: WebSocketConfig[]);
    
    /**
     * Start listening new blocks
     * @returns {Observable<Block>}
     */
    newBlock(): Observable<Block>;
    
    /**
     * Start listening new blockchain height
     * @returns {Observable<BlockHeight>}
     */
    newHeight(): Observable<BlockHeight>;
}


```


# UnconfirmedTransactionListener definition

```typescript
/**
 * UnconfirmedTransaction listener
 */
export declare class UnconfirmedTransactionListener extends Listener {
    
    /**
     * Constructor
     * @param nodes
     */
    constructor(nodes?: WebSocketConfig[]);
    
    /**
     * Start listening new unconfirmed transactions
     * @param address
     * @returns {Observable<Transaction>}
     */
    given(address: Address): Observable<Transaction>;
    
}


```


# ConfirmedTransactionListener definition

```typescript
/**
 * ConfirmedTransaction listener
 */
export declare class ConfirmedTransactionListener extends Listener {
    
    /**
     * Constructor
     * @param nodes
     */
    constructor(nodes?: WebSocketConfig[]);
    
    /**
     * Start listening new confirmed transactions
     * @param address
     * @returns {Observable<Transaction>}
     */
    given(address: Address): Observable<Transaction>;
    
}

```
