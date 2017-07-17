# Account related requests

This chapter will guide you through the process of retrieving account information from a NEM Infrastructure Server. The information that can be retrieved is the durable account data, its meta data and information about transactions and harvested blocks.

NIS supports two different kind of accounts: normal accounts and multsig (short for: multi signature) accounts:

**Normal accounts:**

Normal accounts are created and controlled by a private key. Any action for the account like sending NEM to another account via a transfer transaction is signed with this private key. If an attacker gains knowledge of the private key, he/she can rob the account. The private key must therefore be kept secret by all means.

**Multisig accounts:**

Multisig accounts can be created by converting a normal account to a multisig account via a aggregate modification transaction. This adds cosignatories to the account. After that modification, only the cosignatories can initiate an action for the account. Any action must be signed by all cosignatories. This makes a multisig account significantly more secure than a normal account. When a single cosignatory private key is gained by an attacker, the attacker still can`t initiate any action on the account since all cosignatories must sign. It is strongly recommended to convert any account holding a significantly high amount of NEM into a multisig account with at least 3 cosignatories. Once converted to a multisig account, the original private key for the account plays no role any more.

[Official Source](https://bob.nem.ninja/docs/#account-related-requests)

## AccountHttp definition

```typescript
export declare class AccountHttp extends HttpEndpoint {
    constructor(serverConfig?: ServerConfig);

    /**
     * Gets an AccountInfoWithMetaData for an account.
     * @param address - Address
     * @return Observable<AccountInfoWithMetaData>
     */
    getFromAddress(address: Address): Observable<AccountInfoWithMetaData>;

    /**
     * Gets an AccountInfoWithMetaData for an account with publicKey
     * @param publicKey - NEM
     * @return Observable<AccountInfoWithMetaData>
     */
    getFromPublicKey(publicKey: string): Observable<AccountInfoWithMetaData>;

    /**
     * Given a delegate (formerly known as remote) account`s address, gets the AccountMetaDataPair for the account for which the given account is the delegate account.
     * If the given account address is not a delegate account for any account, the request returns the AccountMetaDataPair for the given address.
     * @param address - Address
     * @return Observable<AccountInfoWithMetaData>
     */
    getOriginalAccountDataFromDelegatedAccountAddress(address: Address): Observable<AccountInfoWithMetaData>;

    /**
     * retrieve the original account data by providing the public key of the delegate account.
     * @param publicKey - string
     * @return Observable<AccountInfoWithMetaData>
     */
    getOriginalAccountDataFromDelegatedAccountPublicKey(publicKey: string): Observable<AccountInfoWithMetaData>;

    /**
     * Gets the AccountMetaData from an account.
     * @param address - NEM Address
     * @return Observable<AccountStatus>
     */
    status(address: Address): Observable<AccountStatus>;

    /**
     * A transaction is said to be incoming with respect to an account if the account is the recipient of the transaction.
     * In the same way outgoing transaction are the transactions where the account is the sender of the transaction.
     * Unconfirmed transactions are those transactions that have not yet been included in a block.
     * Unconfirmed transactions are not guaranteed to be included in any block
     * @param address - The address of the account.
     * @param hash - (Optional) The 256 bit sha3 hash of the transaction up to which transactions are returned.
     * @param id - (Optional) The transaction id up to which transactions are returned. This parameter will prevail over hash.
     * @return Observable<Transaction[]>
     */
    incomingTransactions(address: Address, hash?: string, id?: string): Observable<Transaction[]>;

    /**
     * Gets an array of transaction meta data pairs where the recipient has the address given as parameter to the request.
     * A maximum of 25 transaction meta data pairs is returned. For details about sorting and discussion of the second parameter see Incoming transactions.
     * @param address - The address of the account.
     * @param hash - (Optional) The 256 bit sha3 hash of the transaction up to which transactions are returned.
     * @param id - (Optional) The transaction id up to which transactions are returned. This parameter will prevail over hash.
     * @return Observable<Transaction[]>
     */
    outgoingTransactions(address: Address, hash?: string, id?: string): Observable<Transaction[]>;

    /**
     * Gets an array of transaction meta data pairs for which an account is the sender or receiver.
     * A maximum of 25 transaction meta data pairs is returned.
     * For details about sorting and discussion of the second parameter see Incoming transactions.
     * @param address - The address of the account.
     * @param hash - (Optional) The 256 bit sha3 hash of the transaction up to which transactions are returned.
     * @param id - (Optional) The transaction id up to which transactions are returned. This parameter will prevail over hash.
     * @return Observable<Transaction[]>
     */
    allTransactions(address: Address, hash?: string, id?: string): Observable<Transaction[]>;

    /**
     * Gets the array of transactions for which an account is the sender or receiver and which have not yet been included in a block
     * @param address - NEM Address
     * @return Observable<Transaction[]>
     */
    unconfirmedTransactions(address: Address): Observable<Transaction[]>;

    /**
     * Gets an array of harvest info objects for an account.
     * @param address - Address
     * @param hash - string
     * @return Observable<AccountHarvestInfo[]>
     */
    getHarvestInfoDataForAnAccount(address: Address, hash: string): Observable<AccountHarvestInfo[]>;

    /**
     * Gets an array of account importance view model objects.
     * @return Observable<AccountImportanceInfo[]>
     */
    getAccountImportances(): Observable<AccountImportanceInfo[]>;

    /**
     * Gets an array of namespace objects for a given account address.
     * The parent parameter is optional. If supplied, only sub-namespaces of the parent namespace are returned.
     * @param address - Address
     * @param parent - The optional parent namespace id.
     * @param id - The optional namespace database id up to which namespaces are returned.
     * @param pageSize - The (optional) number of namespaces to be returned.
     * @return Observable<Namespace[]>
     */
    getNamespaceOwnedByAddress(address: Address, parent?: string, id?: string, pageSize?: number): Observable<Namespace[]>;

    /**
     * Gets an array of mosaic definition objects for a given account address. The parent parameter is optional.
     * If supplied, only mosaic definitions for the given parent namespace are returned.
     * The id parameter is optional and allows retrieving mosaic definitions in batches of 25 mosaic definitions.
     * @param address - The address of the account.
     * @param parent - The optional parent namespace id.
     * @param id - The optional mosaic definition database id up to which mosaic definitions are returned.
     * @return Observable<MosaicDefinition[]>
     */
    getMosaicCreatedByAddress(address: Address, parent?: string, id?: string): Observable<MosaicDefinition[]>;

    /**
     * Gets an array of mosaic objects for a given account address.
     * @param address - Address
     * @return Observable<Mosaic[]>
     */
    getMosaicOwnedByAddress(address: Address): Observable<Mosaic[]>;

    /**
     * Unlocks an account (starts harvesting)
     * @param privateKey - string
     * @return Observable<boolean>
     */
    unlockHarvesting(privateKey: string): Observable<boolean>;

    /**
     * Locks an account (stops harvesting).
     * @param privateKey - string
     * @return Observable<boolean>
     */
    lockHarvesting(privateKey: string): Observable<boolean>;

    /**
     * Each node can allow users to harvest with their delegated key on that node.
     * The NIS configuration has entries for configuring the maximum number of allowed harvesters and optionally allow harvesting only for certain account addresses.
     * The unlock info gives information about the maximum number of allowed harvesters and how many harvesters are already using the node.
     * @return Observable<NodeHarvestInfo>
     */
    unlockInfo(): Observable<NodeHarvestInfo>;

    /**
     * Gets historical information for an account.
     * @param address - The address of the account.
     * @param startHeight - The block height from which on the data should be supplied.
     * @param endHeight - The block height up to which the data should be supplied. The end height must be greater than or equal to the start height.
     * @param increment - The value by which the height is incremented between each data point. The value must be greater than 0. NIS can supply up to 1000 data points with one request. Requesting more than 1000 data points results in an error.
     * @return Observable<AccountHistoricalInfo[]>
     */
    getHistoricalAccountData(address: Address, startHeight: number, endHeight: number, increment: number): Observable<AccountHistoricalInfo[]>;
}
```

## AccountHttp usage

```typescript
import {AccountHttp, NEMLibrary, NetworkTypes, Address} from "nem-library";

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const address = new Address("");

const accountHttp = new AccountHttp({domain: "104.128.226.60"});
accountHttp.getFromAddress(address).subscribe(accountInfoWithMetaData => {
    console.log(accountInfoWithMetaData);
});
```