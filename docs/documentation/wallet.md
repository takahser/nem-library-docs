# Wallets



# SimpleWallet definition

```typescript
/**
 * Simple wallet model generates a private key from a PRNG
 */
export declare class SimpleWallet extends Wallet {
    
    /**
     * The encripted private key and information to decrypt it
     */
    readonly encryptedPrivateKey: EncryptedPrivateKey;
    
    /**
     * Create a SimpleWallet
     * @param name
     * @param password
     * @returns {SimpleWallet}
     */
    static create(name: string, password: Password): SimpleWallet;
    
    /**
     * Create a SimpleWallet from private key
     * @param name
     * @param network
     * @param password
     * @param privateKey
     * @returns {SimpleWallet}
     */
    static createWithPrivateKey(name: string, password: Password, privateKey: string): SimpleWallet;
    
    /**
     * Open a wallet and generate an Account
     * @param password
     * @returns {Account}
     */
    open(password: Password): Account;

    /**
    * Converts SimpleWallet into writable string to persist into a file
    * @returns {string}
    */
    writeWLTFile(): string;

    /**
    * Reads the WLT content and converts it into a SimpleWallet
    * @param {string} wlt
    * @returns {SimpleWallet}
    */
    static readFromWLT(wlt: string): SimpleWallet;
}

```

# SimpleWallet usage

```typescript
/**
 * nem-library 0.3.0
 */

import {SimpleWallet, Password, NetworkTypes, NEMLibrary} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const password = new Password("password");
const simpleWallet = SimpleWallet.create("simple wallet", password);

console.log(simpleWallet);
```

Output

```
SimpleWallet {
  name: 'simple wallet',
  network: 152,
  address: 
   Address {
     value: 'TBSICCK3PJZDWQ4JCPWT55IRBWEQMRSABQ3QJHE4',
     networkType: 152 },
  creationDate: 
   LocalDateTime {
     _date: LocalDate { _year: 2017, _month: 7, _day: 24 },
     _time: LocalTime { _hour: 16, _minute: 44, _second: 14, _nano: 870000000 } },
  encryptedPrivateKey: 
   EncryptedPrivateKey {
     encryptedKey: '2cb583e61208964d465955cc35c86c9e83733a10adc40cee6b8c650478ffe3667ccb70328a0dc2c67de9770806040580',
     iv: 'c192d9a501c7dde5c7e05186c93b63da' } }

```

[Run the code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/concepts/wallet/SimpleWalletExample.ts)


# BrainWallet definition

```typescript
/**
 * Brain wallet derived the private key from the brainPassword, hashing the brainPassword multiple times, therefore it's crucial to select a SAFE brainPassword.
 */
export declare class BrainWallet extends Wallet {
    
    /**
     * Create a BrainWallet
     * @param name
     * @param password
     * @returns {BrainWallet}
     */
    static create(name: string, password: BrainPassword): BrainWallet;
    
    /**
     * Open a wallet and generate an Account
     * @param password
     * @returns {Account}
     */
    open(password: BrainPassword): Account;

    /**
    * Converts BrainWallet into writable string to persist into a file
    * @returns {string}
    */
    writeWLTFile(): string;

    /**
    * Reads the WLT content and converts it into a BrainWallet
    * @param {string} wlt
    * @returns {BrainWallet}
    */
    static readFromWLT(wlt: string): BrainWallet;
}

```

# BrainWallet usage

```typescript
/**
 * nem-library 0.3.0
 */

import {BrainWallet, BrainPassword, NetworkTypes, NEMLibrary} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const brainPassword =  new BrainPassword("entertain destruction sassy impartial morning electric limit glib bait grape icy measure")
const brainWallet = BrainWallet.create("brain wallet", brainPassword);

console.log(brainWallet);
```
Output

```
BrainWallet {
  name: 'brain wallet',
  network: 152,
  address: 
   Address {
     value: 'TBUWTIIYM2BFFAE3JOEW3LGG5X3QTO7L2RWGZ6XV',
     networkType: 152 },
  creationDate: 
   LocalDateTime {
     _date: LocalDate { _year: 2017, _month: 7, _day: 24 },
     _time: LocalTime { _hour: 16, _minute: 44, _second: 42, _nano: 402000000 } } }

```

[Run the code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/concepts/wallet/BrainWalletExample.ts)


# Models

## Wallet

```typescript
export enum WalletType {
  SIMPLE,
  BRAIN
}

/**
 * Wallet base model
 */
export declare abstract class Wallet {
    
    /**
     * The wallet's name
     */
    readonly name: string;
    
    /**
     * The wallet's network
     */
    readonly network: NetworkTypes;
    
    /**
     * The wallet's address
     */
    readonly address: Address;
    
    /**
     * The wallet's creation date
     */
    readonly creationDate: LocalDateTime;
    
    /**
     * Abstract open wallet method returning an account from current wallet.
     * @param password
     */
    abstract open(password: Password): Account;

    /**
    * Given a WLT string, retusn the WalletType
    * @param {string} wlt
    * @returns {WalletType}
    */
    static walletTypeGivenWLT(wlt: string): WalletType;
}

```
## Password

```typescript
/**
 * Password model
 */
export declare class Password {
    
    /**
     * Password value
     */
    readonly value: string;
    
    /**
     * Create a password with at least 8 characters
     * @param password
     */
    constructor(password: string);
}

```

## BrainPassword

```typescript
/**
 * Brain password is an extended version of Password. With the brain password we derive the private key in BrainWallets.
 */
export declare class BrainPassword extends Password {
    
    /**
     * Constructor
     * @param password - password must be secure, the password must be at least a 12 random words password.
     */
    constructor(password: string);
}


```

## EncryptedPrivateKey

```typescript
/**
 * EncryptedPrivateKey model
 */
export declare class EncryptedPrivateKey {
    
    /**
     * Encrypted private key data
     */
    readonly encryptedKey: string;
    
    /**
     * Initialization vector used in the decrypt process
     */
    readonly iv: string;
}
```
