# Initiating transactions requests

Transactions are the way of transferring NEM and/or messages from one account to another. Once a transaction is initiated, it is still unconfirmed and thus not yet accepted by the network. At this point it is not yet clear if it will get included in a block. Never rely on a transaction which has the state 'unconfirmed'. Once it is included in a block, the transaction gets processed and, in case of a transfer transaction, the amount stated in the transaction gets transferred from the sender's account to the recipient's account. Additionally the transaction fee is deducted from the sender's account. The transaction is said to have 0 confirmations at this point. When another block is added to the block chain the transaction has 1 confirmation. The next block added to the chain will give it 2 confirmations and so on.

Crypto currencies have the ability to roll back part the block chain. This is essential for being able to resolve forks of the block chain. There is however a maximum number of blocks that can be rolled back, this is called the rewrite limit. Hence forks can only be resolved up to a certain depth too. NEM has a rewrite limit of 360 blocks. Once a transaction has more than 360 confirmations, it cannot be reversed. In real life, forks that are deeper than 20 blocks do not happen, unless there was some severe problem with the block chain due to a bug in the code or an attack of some kind.

[Official Source](https://bob.nem.ninja/docs/#initiating-transactions)

# TransactionHttp definition

```typescript
export declare class TransactionHttp extends HttpEndpoint {
    constructor(nodes?: NisConfig[]);
    
    /**
     * Send the signed transaction
     * @param transaction
     * @returns Observable<NemAnnounceResult>
     */
    announceTransaction(transaction: SignedTransaction): Observable<NemAnnounceResult>;

    /**
    * Receive a transaction by its hash
    * @param {string} hash - transaction hash
    * @returns Observable<Transaction>
    */
    getByHash(hash: string): Observable<Transaction>;
}
```

# TransactionHttp usage

```typescript
import {
    AccountHttp, NEMLibrary, NetworkTypes, Address, Account, TransferTransaction, TimeWindow,
    EmptyMessage, MultisigTransaction, PublicAccount, TransactionHttp, SignedTransaction
} from "nem-library";
import {XEM} from "nem-library/dist/src/models/mosaic/XEM";
declare let process: any;

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const privateKey: string = process.env.PRIVATE_KEY;
const multisigAccountPublicKey: string = process.env.MULTISIG_PUBLIC_KEY;

const cosignerAccount = Account.createWithPrivateKey(privateKey);

const transferTransaction = TransferTransaction.create(
    TimeWindow.createWithDeadline(),
    new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"),
    new XEM(2),
    EmptyMessage
);

const multisigTransaction = MultisigTransaction.create(
    TimeWindow.createWithDeadline(),
    transferTransaction,
    PublicAccount.createWithPublicKey(multisigAccountPublicKey)
);

const transactionHttp = new TransactionHttp();

const signedTransaction: SignedTransaction = cosignerAccount.signTransaction(multisigTransaction);

transactionHttp.announceTransaction(signedTransaction).subscribe( x => console.log(x));
```

Output

```

NemAnnounceResult {
  type: 1,
  code: 1,
  message: 'SUCCESS',
  transactionHash: { data: '56b4d3e38cb5b707d4b96776116396c2885fd09be6945637f5657204528001b7' },
  innerTransactionHash: { data: 'a61ce8d0df9aac98ae68e7d88a2f2bb453deb202fd95cf3cf375fde35e4e6794' } }
  
```

[Run the code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/concepts/transaction/TrasactionHttpExample.ts)


# Models

## TimeWindow

```typescript
export declare class TimeWindow {
    static timestampNemesisBlock: number;
    
    /**
     * The deadline of the transaction. The deadline is given as the number of seconds elapsed since the creation of the nemesis block.
     * If a transaction does not get included in a block before the deadline is reached, it is deleted.
     */
    deadline: LocalDateTime;
    
    /**
     * The number of seconds elapsed since the creation of the nemesis block.
     */
    timeStamp: LocalDateTime;
    
    /**
     * @param deadline
     * @param chronoUnit
     * @returns {TimeWindow}
     */
    static createWithDeadline(deadline?: number, chronoUnit?: ChronoUnit): TimeWindow;
}

```

## TransactionInfo

```typescript
export declare class TransactionInfo {
    
    /**
     * The height of the block in which the transaction was included.
     */
    readonly height: number;
    
    /**
     *  The id of the transaction.
     */
    readonly id: number;
    
    /**
     *  The transaction hash.
     */
    readonly hash: HashData;
    
    /**
     * constructor
     * @param height
     * @param id
     * @param hash
     */
    constructor(height: number, id: number, hash: HashData);
}

export declare class MultisigTransactionInfo extends TransactionInfo {
    
    /**
     * The hash of the inner transaction. This entry is only available for multisig transactions.
     */
    readonly innerHash: HashData;
    
    /**
     * constructor
     * @param height
     * @param id
     * @param hash
     * @param innerHash
     */
    constructor(height: number, id: number, hash: HashData, innerHash: HashData);
}

```

## TransactionTypes

```typescript
/**
 * Static class containing transaction type constants.
 */
export declare class TransactionTypes {
    
    /**
     * Transfer Transaction
     * @type {number}
     */
    static readonly TRANSFER: number;
    
    /**
     * Importance transfer transaction.
     * @type {number}
     */
    static readonly IMPORTANCE_TRANSFER: number;
    
    /**
     * A new asset transaction.
     * @type {number}
     */
    static readonly ASSET_NEW: number;
    
    /**
     * An asset ask transaction.
     * @type {number}
     */
    static readonly ASSET_ASK: number;
    
    /**
     * An asset bid transaction.
     * @type {number}
     */
    static readonly ASSET_BID: number;
    
    /**
     * A snapshot transaction.
     * @type {number}
     */
    static readonly SNAPSHOT: number;
    
    /**
     * A multisig change transaction (e.g. announce an account as multi-sig).
     * @type {number}
     */
    static readonly MULTISIG_AGGREGATE_MODIFICATION: number;
    
    /**
     * A multisig signature transaction.
     * @type {number}
     */
    static readonly MULTISIG_SIGNATURE: number;
    
    /**
     * A multisig transaction.
     * @type {number}
     */
    static readonly MULTISIG: number;
    
    /**
     * A provision namespace transaction.
     * @type {number}
     */
    static readonly PROVISION_NAMESPACE: number;
    
    /**
     * A mosaic definition creation transaction.
     * @type {number}
     */
    static readonly MOSAIC_DEFINITION_CREATION: number;
    
    /**
     * A mosaic supply change transaction.
     * @type {number}
     */
    static readonly MOSAIC_SUPPLY_CHANGE: number;
    
    /**
     * Gets all multisig embeddable types.
     * @returns {number[]}
     */
    static getMultisigEmbeddableTypes(): number[];
    
    /**
     * Gets all block embeddable types.
     * @returns {number[]}
     */
    static getBlockEmbeddableTypes(): number[];
    
    /**
     * Gets all active types.
     * @returns {number[]}
     */
    static getActiveTypes(): number[];
}

```

## Transaction

```typescript
/**
 * An abstract transaction class that serves as the base class of all NEM transactions.
 */
export declare abstract class Transaction {
    
    /**
     * The transaction type.
     */
    readonly type: number;
    
    /**
     * The version of the structure.
     */
    readonly version: number;
    
    /**
     * The transaction signature (missing if part of a multisig transaction).
     */
    readonly signature?: string;
    
    /**
    * The public account of the transaction creator.
    */
    public signer?: PublicAccount;
    
    /**
     * TimeWindow
     */
    readonly timeWindow: TimeWindow;
    
    /**
     * The fee for the transaction. The higher the fee, the higher the priority of the transaction. Transactions with high priority get included in a block before transactions with lower priority.
     */
    readonly abstract fee: number;
    
    /**
     * Transactions meta data object contains additional information about the transaction.
     */
    protected readonly transactionInfo?: TransactionInfo;
    
    /**
     * Checks if the transaction has been confirmed and included in a block
     */
    isConfirmed(): boolean;
    
    /**
     * Get transaction info
     */
    getTransactionInfo(): TransactionInfo;
}

```



## TransferTransaction

```typescript
/**
 * Transfer transactions contain data about transfers of XEM or mosaics to another account.
 */
export declare class TransferTransaction extends Transaction {
    /**
     * The fee for the transaction. The higher the fee, the higher the priority of the transaction. Transactions with high priority get included in a block before transactions with lower priority.
     */
    readonly fee: number;
    /**
     * The address of the recipient.
     */
    readonly recipient: Address;
    /**
     * The xem of XEM that is transferred from sender to recipient.
     */
    private readonly _xem;
    /**
     * Optionally a transaction can contain a message. In this case the transaction contains a message substructure. If not the field is null.
     */
    readonly message: PlainMessage | EncryptedMessage;
    /**
     * The array of Mosaic objects.
     */
    private readonly _mosaics?;
    /**
     * in case that the transfer transaction contains mosaics, it throws an error
     * @returns {XEM}
     */
    xem(): XEM;
    /**
     * in case that the transfer transaction does not contain mosaics, it throws an error
     * @returns {Mosaic[]}
     */
    mosaics(): Mosaic[];
    /**
     *
     * @returns {boolean}
     */
    containsMosaics(): boolean;
    /**
     * all the Mosaic Identifiers of the attached mosaics
     * @returns {MosaicId[]}
     */
    mosaicIds(): MosaicId[];
    /**
     * Create a TransferTransaction object
     * @param timeWindow
     * @param recipient
     * @param xem
     * @param message
     * @returns {TransferTransaction}
     */
    static create(timeWindow: TimeWindow, recipient: Address, xem: XEM, message: PlainMessage | EncryptedMessage): TransferTransaction;
    /**
     * Create a TransferTransaction object
     * @param timeWindow
     * @param recipient
     * @param mosaics
     * @param message
     * @returns {TransferTransaction}
     */
    static createWithMosaics(timeWindow: TimeWindow, recipient: Address, mosaics: MosaicTransferable[], message: PlainMessage | EncryptedMessage): TransferTransaction;
}
```

## Message

```typescript
/**
 * Message model
 */
export declare abstract class Message {
    
    /**
     * Message payload
     */
    readonly payload: string;
    
}

```

## PlainMessage

```typescript
/**
 * Plain Message model
 */
export declare class PlainMessage extends Message {
        
    /**
     * Create new constructor
     * @returns {boolean}
     */
    static create(message: string): PlainMessage;
    
    /**
     * Message string
     * @returns {string}
     */
    plain(): string;
    
}
export declare const EmptyMessage: PlainMessage;

```

## EncryptedMessage

```typescript
/**
 * Encrypted Message model
 */
export declare class EncryptedMessage extends Message {
    readonly recipientPublicAccount?: PublicAccount;
}


```


## ImportanceTransferTransaction

```typescript

export declare enum ImportanceMode {
    Activate = 1,
    Deactivate = 2,
}

/**
 * NIS has the ability to transfer the importance of one account to another account for harvesting.
 * The account receiving the importance is called the remote account.
 * Importance transfer transactions are part of the secure harvesting feature of NEM.
 * Once an importance transaction has been included in a block it needs 6 hours to become active.
 */
export declare class ImportanceTransferTransaction extends Transaction {
    
    /**
     * The fee for the transaction. The higher the fee, the higher the priority of the transaction. Transactions with high priority get included in a block before transactions with lower priority.
     */
    readonly fee: number;
    
    /**
     * The public key of the receiving account as hexadecimal string.
     */
    readonly remoteAccount: PublicAccount;
    
    /**
     * The mode, activate or deactivate
     */
    readonly mode: ImportanceMode;
    
    /**
     * Create a ImportanceTransferTransaction object
     * @param timeWindow
     * @param mode
     * @param remoteAccount
     * @returns {ImportanceTransferTransaction}
     */
    static create(timeWindow: TimeWindow, mode: ImportanceMode, remoteAccount: PublicAccount): ImportanceTransferTransaction;
}

```

## ProvisionNamespaceTransaction

```typescript
/**
 * Accounts can rent a namespace for one year and after a year renew the contract. This is done via a ProvisionNamespaceTransaction.
 */
export declare class ProvisionNamespaceTransaction extends Transaction {
    
    /**
     * The fee for the transaction. The higher the fee, the higher the priority of the transaction. Transactions with high priority get included in a block before transactions with lower priority.
     */
    readonly fee: number;
    
    /**
     * The Address to which the rental fee is transferred.
     */
    readonly rentalFeeSink: Address;
    
    /**
     * The fee for renting the namespace.
     */
    readonly rentalFee: number;
    
    /**
     * The parent namespace. This can be undefined if the transaction rents a root namespace.
     */
    readonly parent?: string;
    
    /**
     * The new part which is concatenated to the parent with a '.' as separator.
     */
    readonly newPart: string;
    
    /**
     * Create a ProvisionNamespaceTransaction object
     * @param timeWindow
     * @param newPart
     * @param parent
     * @returns {ProvisionNamespaceTransaction}
     */
    static create(timeWindow: TimeWindow, newPart: string, parent?: string): ProvisionNamespaceTransaction;

    /**
   *
   * @param {TimeWindow} timeWindow
   * @param {string} namespaceName - Root namespace provision
   * @returns {ProvisionNamespaceTransaction}
   */
  static createRoot(timeWindow: TimeWindow, namespaceName: string): ProvisionNamespaceTransaction;

  /**
   *
   * @param {TimeWindow} timeWindow
   * @param {string }parentNamespace
   * @param {string} newNamespaceName
   * @returns {ProvisionNamespaceTransaction}
   */
  static createSub(timeWindow: TimeWindow, parentNamespace: string, newNamespaceName: string): ProvisionNamespaceTransaction;
}
```


## MosaicDefinitionCreationTransaction

```typescript
/**
 * Before a mosaic can be created or transferred, a corresponding definition of the mosaic has to be created and published to the network.
 * This is done via a mosaic definition creation transaction.
 */
export declare class MosaicDefinitionCreationTransaction extends Transaction {
    
    /**
     * The fee for the transaction. The higher the fee, the higher the priority of the transaction. Transactions with high priority get included in a block before transactions with lower priority.
     */
    readonly fee: number;
    
    /**
     * The fee for the creation of the mosaic.
     */
    readonly creationFee: number;
    
    /**
     * The public account to which the creation fee is tranferred.
     */
    readonly creationFeeSink: Address;
    
    /**
     * The actual mosaic definition.
     */
    readonly mosaicDefinition: MosaicDefinition;
    
    /**
     * Create a MosaicDefinitionCreationTransaction object
     * @param timeWindow
     * @param mosaicDefinition
     * @returns {MosaicDefinitionCreationTransaction}
     */
    static create(timeWindow: TimeWindow, mosaicDefinition: MosaicDefinition): MosaicDefinitionCreationTransaction;
}

```

## MosaicSupplyChangeTransaction

```typescript
export declare enum MosaicSupplyType {
    Increase = 1,
    Decrease = 2,
}

/**
 * In case a mosaic definition has the property 'supplyMutable' set to true, the creator of the mosaic definition can change the supply, i.e. increase or decrease the supply.
 */
export declare class MosaicSupplyChangeTransaction extends Transaction {
    
    /**
     * The fee for the transaction. The higher the fee, the higher the priority of the transaction. Transactions with high priority get included in a block before transactions with lower priority.
     */
    readonly fee: number;
    
    /**
     * The mosaic id.
     */
    readonly mosaicId: MosaicId;
    
    /**
     * The supply type.
     */
    readonly supplyType: MosaicSupplyType;
    
    /**
     * The supply change in units for the mosaic.
     */
    readonly delta: number;
    
    /**
     * Create a MosaicSupplyChangeTransaction object
     * @param timeWindow
     * @param mosaicId
     * @param supplyType
     * @param delta
     * @returns {MosaicSupplyChangeTransaction}
     */
    static create(timeWindow: TimeWindow, mosaicId: MosaicId, supplyType: MosaicSupplyType, delta: number): MosaicSupplyChangeTransaction;
}

```


## MultisigTransaction

```typescript
/**
 * Multisig transaction are the only way to make transaction from a multisig account to another account.
 * A multisig transaction carries another transaction inside (often referred to as "inner" transaction).
 * The inner transaction can be a transfer, an importance transfer or an aggregate modification transaction.
 * A multisig transaction also has multisig signature transactions from the cosignatories of the multisig account inside.
 */
export declare class MultisigTransaction extends Transaction {
    
    /**
     * The fee for the transaction.
     */
    readonly fee: number;
    
    /**
     * The JSON array of MulsigSignatureTransaction objects.
     */
    readonly signatures: MultisigSignatureTransaction[];
    
    /**
     * The inner transaction. The inner transaction can be a transfer transaction, an importance transfer transaction or a multisig aggregate modification transaction.
     * The inner transaction does not have a valid signature.
     */
    readonly otherTransaction: Transaction;
    
    /**
     * Hash data
     */
    readonly hashData?: HashData;
    
    /**
     * Check if transaction is pending to sign
     * @returns {boolean}
     */
    isPendingToSign(): boolean;
    
    /**
     * Create a MultisigTransaction object
     * @param timeWindow
     * @param otherTrans
     * @param multisig
     * @returns {MultisigTransaction}
     */
    static create(timeWindow: TimeWindow, otherTrans: Transaction, multisig: PublicAccount): MultisigTransaction;
}

```

## MultisigAggregateModificationTransaction

```typescript
/**
 * Multisig aggregate modification transactions are part of the NEM's multisig account system.
 * A multisig aggregate modification transaction holds an array of multisig cosignatory modifications and a single multisig minimum cosignatories modification inside the transaction.
 * A multisig aggregate modification transaction can be wrapped by a multisig transaction.
 */
export declare class MultisigAggregateModificationTransaction extends Transaction {
    
    /**
     * The fee for the transaction. The higher the fee, the higher the priority of the transaction. Transactions with high priority get included in a block before transactions with lower priority.
     */
    fee: number;
    
    /**
     * Value indicating the relative change of the minimum cosignatories.
     */
    readonly relativeChange?: number;
    
    /**
     * The JSON array of multisig modifications.
     */
    readonly modifications: CosignatoryModification[];
    
    /**
     * Create a MultisigAggregateModificationTransaction object
     * @param timeWindow
     * @param modifications
     * @param relativeChange
     * @returns {MultisigAggregateModificationTransaction}
     */
    static create(timeWindow: TimeWindow, modifications: CosignatoryModification[], relativeChange?: number): MultisigAggregateModificationTransaction;
}

/**
 * The type of modification. Possible values are:
 * 1: Add a new cosignatory.
 * 2: Delete an existing cosignatory.
 */
export declare enum CosignatoryModificationAction {
    ADD = 1,
    DELETE = 2,
}

export declare class CosignatoryModification {
    readonly cosignatoryAccount: PublicAccount;
    readonly action: CosignatoryModificationAction;
    
    /**
     * constructor
     * @param cosignatoryAccount
     * @param action
     */
    constructor(cosignatoryAccount: PublicAccount, action: CosignatoryModificationAction);
}

```

## MultisigSignatureTransaction

```typescript
/**
 * Multisig signature transactions are part of the NEM's multisig account system. Multisig signature transactions are included in the corresponding multisig transaction and are the way a cosignatory of a multisig account can sign a multisig transaction for that account.
 */
export declare class MultisigSignatureTransaction extends Transaction {
    
    /**
     * The fee for the transaction. The higher the fee, the higher the priority of the transaction. Transactions with high priority get included in a block before transactions with lower priority.
     */
    readonly fee: number;
    
    /**
     * The address of the corresponding multisig account.
     */
    readonly otherAccount: Address;
    
    /**
     * The hash of the inner transaction of the corresponding multisig transaction.
     */
    readonly otherHash: HashData;
    
    /**
     * Create a MultisigSignatureTransaction object
     * @param timeWindow
     * @param otherAccount
     * @param otherHash
     * @returns {MultisigSignatureTransaction}
     */
    static create(timeWindow: TimeWindow, otherAccount: Address, otherHash: HashData): MultisigSignatureTransaction;
}

```

## SignedTransaction

```typescript
/**
 * SignedTransaction object is used to transfer the transaction data and the signature to NIS in order to initiate and broadcast a transaction.
 */
export interface SignedTransaction {
    
    /**
     * The transaction data as string.
     */
    readonly data: string;
    
    /**
     * The signature for the transaction as hexadecimal string.
     */
    readonly signature: string;
}

```

## NemAnnounceResult

```typescript

export declare enum TypeNemAnnounceResult {
    Validation = 1,
    HeartBeat = 2,
    Status = 4,
}

export declare type CodeNemAnnounceResult = 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19;

/**
 * The NemAnnounceResult extends the NemRequestResult by supplying the additional fields 'transactionHash' and in case of a multisig transaction 'innerTransactionHash'.
 */
export declare class NemAnnounceResult {
    
    /**
     * The type is dependent on the request which was answered.
     */
    readonly type: TypeNemAnnounceResult;
    
    /**
     * The meaning of the code is dependent on the type.
     */
    readonly code: CodeNemAnnounceResult;
    
    /**
     * Error or success message
     */
    readonly message: string;
    
    /**
     * The JSON hash object of the transaction.
     */
    readonly transactionHash: HashData;
    
    /**
     * The JSON hash object of the inner transaction or null if the transaction is not a multisig transaction.
     */
    readonly innerTransactionHash: HashData;
}

```
