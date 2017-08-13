### How to create a listener for account information

```typescript
/**
 * nem-library 0.3.0
 */

import {AccountListener, Address, NEMLibrary, NetworkTypes} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const address = new Address("TCJZJH-AV63RE-2JSKN2-7DFIHZ-RXIHAI-736WXE-OJGA");
let listener = new AccountListener().given(address);

listener.subscribe(x => {
    console.log(x);
}, err => {
    console.log(err);
});
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/listener/How_to_create_a_listener_for_account_information.ts)


### How to create a listener for new blocks information

```typescript
/**
 * nem-library 0.3.0
 */
import {BlockchainListener, NEMLibrary, NetworkTypes} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

let blockchainListener = new BlockchainListener().newBlock();

blockchainListener.subscribe(x => {
    console.log(x);
}, err => {
    console.log(err);
});


```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/listener/How_to_create_a_listener_for_new_blocks_information.ts)


### How to create a listener for new blockchain height information

```typescript
/**
 * nem-library 0.3.0
 */
import {BlockchainListener, NEMLibrary, NetworkTypes} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

let blockchainListener = new BlockchainListener().newHeight();

blockchainListener.subscribe(x => {
    console.log(x);
}, err => {
    console.log(err);
});
```


[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/listener/How_to_create_a_listener_for_new_blockchain_height_information.ts)

### How to create a listener for unconfirmed transactions information

```typescript
/**
 * nem-library 0.3.0
 */

import {Address, NEMLibrary, NetworkTypes, UnconfirmedTransactionListener} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const address = new Address("TDM3DO-ZM5WJ3-ZRBPSM-YRU6JS-WKUCAH-5VIPOF-4W7K");

let unconfirmedTransactionListener = new UnconfirmedTransactionListener().given(address);
unconfirmedTransactionListener.subscribe(x => {
    console.log(x);
}, err => {
    console.log(err);
});
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/listener/How_to_create_a_listener_for_unconfirmed_transactions_information.ts)

### How to create a listener for confirmed transactions information

```typescript
/**
 * nem-library 0.3.0
 */

import {Address, ConfirmedTransactionListener, NEMLibrary, NetworkTypes} from "nem-library";

NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const address = new Address("TDM3DO-ZM5WJ3-ZRBPSM-YRU6JS-WKUCAH-5VIPOF-4W7K");

let confirmedTransactionListener = new ConfirmedTransactionListener().given(address);
confirmedTransactionListener.subscribe(x => {
    console.log(x);
}, err => {
    console.log(err);
});
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/listener/How_to_create_a_listener_for_confirmed_transactions_information.ts)
