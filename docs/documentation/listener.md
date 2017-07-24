# Listeners


# AccountListener definition

```typescript
/**
 * Account listener
 */
export declare class AccountListener extends Listener {
    
    /**
     * Constructor
     * @param config
     */
    constructor(config?: WebSocketConfig);
    
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

import {AccountListener, Address} from "nem-library";

const address = new Address("TCJZJH-AV63RE-2JSKN2-7DFIHZ-RXIHAI-736WXE-OJGA");
let accountListener = new AccountListener({domain: "23.228.67.85"}).given(address).subscribe(x => {
    console.log(x);
}, err => {
    console.log(err);
});

```

Output

```

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
     * @param config
     */
    constructor(config?: WebSocketConfig);
    
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
     * @param config
     */
    constructor(config?: WebSocketConfig);
    
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
     * @param config
     */
    constructor(config?: WebSocketConfig);
    
    /**
     * Start listening new confirmed transactions
     * @param address
     * @returns {Observable<Transaction>}
     */
    given(address: Address): Observable<Transaction>;
    
}


```
