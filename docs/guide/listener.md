### How to create a listener for account information

```typescript
/**
 * nem-library 0.3.0
 */

import {AccountListener, Address} from "nem-library";

const address = new Address("TCJZJH-AV63RE-2JSKN2-7DFIHZ-RXIHAI-736WXE-OJGA");
let accountListener = new AccountListener({domain: "23.228.67.85"}).given(address).subscribe(x => {
    console.log(x);
    done();
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

import {BlockchainListener} from "nem-library";

let blockchainListener = new BlockchainListener({domain: "23.228.67.85"}).newBlock().subscribe(x => {
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

import {BlockchainListener} from "nem-library";

let blockchainListener = new BlockchainListener({domain: "23.228.67.85"}).newHeight().subscribe(x => {
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

import {Address, UnconfirmedTransactionListener} from "nem-library";

const address = new Address("TDM3DO-ZM5WJ3-ZRBPSM-YRU6JS-WKUCAH-5VIPOF-4W7K");

let unconfirmedTransactionListener = new UnconfirmedTransactionListener({domain: "23.228.67.85"}).given(address).subscribe(x => {
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

import {Address, ConfirmedTransactionListener} from "nem-library";

const address = new Address("TDM3DO-ZM5WJ3-ZRBPSM-YRU6JS-WKUCAH-5VIPOF-4W7K");

let confirmedTransactionListener = new ConfirmedTransactionListener({domain: "23.228.67.85"}).given(address).subscribe(x => {
    console.log(x);
}, err => {
    console.log(err);
});
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/listener/How_to_create_a_listener_for_confirmed_transactions_information.ts)
