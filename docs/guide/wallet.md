### How to create a Simple Wallet

```typescript
/**
 * nem-library 0.3.0
 */

import {SimpleWallet, Password, NetworkTypes, NEMLibrary} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const password = new Password("password");
const simpleWallet = SimpleWallet.create("simple wallet", password);
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/wallet/How_to_create_a_Simple_Wallet.ts)

### How to create a Brain Wallet

```typescript
/**
 * nem-library 0.3.0
 */

import {BrainWallet, BrainPassword, NetworkTypes, NEMLibrary} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const brainPassword =  new BrainPassword("entertain destruction sassy impartial morning electric limit glib bait grape icy measure")
const brainWallet = BrainWallet.create("brain wallet", brainPassword);
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/wallet/How_to_create_a_Brain_Wallet.ts)

### How to create a Simple Wallet from a private key

```typescript
/**
 * nem-library 0.3.0
 */

import {SimpleWallet, Password, NetworkTypes, NEMLibrary} from "nem-library";
declare let process: any;

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const privateKey: string = process.env.PRIVATE_KEY;


const password = new Password("password");
const simpleWallet = SimpleWallet.createWithPrivateKey("simple wallet", password, privateKey);
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/wallet/How_to_create_a_Simple_Wallet_from_a_private_key.ts)

### How to open a Wallet

```typescript
/**
 * nem-library 0.3.0
 */

import {SimpleWallet, Password, NetworkTypes, NEMLibrary} from "nem-library";
declare let process: any;

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const privateKey: string = process.env.PRIVATE_KEY;


const password = new Password("password");
const simpleWallet = SimpleWallet.createWithPrivateKey("simple wallet", password, privateKey);
const account = simpleWallet.open(password);
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/wallet/How_to_open_a_Wallet.ts)
