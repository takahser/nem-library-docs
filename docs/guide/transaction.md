### How to create a Transfer Transaction

```typescript
import {
    NEMLibrary, NetworkTypes, Address, TransferTransaction, Transaction, TimeWindow,
    EmptyMessage, MultisigTransaction, PublicAccount, TransactionHttp, XEM
} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const transferTransaction: Transaction = TransferTransaction.create(
    TimeWindow.createWithDeadline(),
    new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"),
    new XEM(2),
    EmptyMessage
);
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/transaction/How_to_create_a_Transfer_Transaction.ts)

### How to sign a Transaction

```typescript
import {
    AccountHttp, NEMLibrary, NetworkTypes, Address, Account, TransferTransaction, TimeWindow,
    EmptyMessage, MultisigTransaction, PublicAccount, TransactionHttp, XEM
} from "nem-library";
declare let process: any;

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const transactionHttp = new TransactionHttp({domain: "104.128.226.60"});

// Replace with a cosignatory private key
const privateKey: string = process.env.PRIVATE_KEY;

const account = Account.createWithPrivateKey(privateKey);

const transferTransaction = TransferTransaction.create(
    TimeWindow.createWithDeadline(),
    new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"),
    new XEM(2),
    EmptyMessage
);

const signedTransaction = account.signTransaction(transferTransaction);

transactionHttp.announceTransaction(signedTransaction).subscribe( x => console.log(x));
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/transaction/How_to_sign_a_Transaction.ts)

### How to create a Transfer Transaction with a Message

```typescript
/**
 * nem-library 0.3.0
 */
import {
    NEMLibrary, NetworkTypes, Address, TransferTransaction, Transaction, TimeWindow,
    XEM, PlainMessage
} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const transferTransaction: Transaction = TransferTransaction.create(
    TimeWindow.createWithDeadline(),
    new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"),
    new XEM(0),
    PlainMessage.create("a transaction")
);

```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/transaction/How_to_create_a_Transfer_Transaction_with_a_Message.ts)


### How to create a Transfer Transaction with an Encrypted Message

```typescript
/**
 * nem-library 0.3.0
 */
import {
    NEMLibrary, NetworkTypes, Account, TransferTransaction, TimeWindow,
    TransactionHttp, XEM, PublicAccount
} from "nem-library";
declare let process: any;

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const transactionHttp = new TransactionHttp({domain: "104.128.226.60"});

// Replace with a cosignatory private key
const privateKey: string = process.env.PRIVATE_KEY;
const recipientPublicAccount = PublicAccount.createWithPublicKey("b254d8b2b00e1b1266eb54a6931cd7c1b0f307e41d9ebb01f025f4933758f0be");

const account = Account.createWithPrivateKey(privateKey);

const encryptedMessage = account.encryptMessage("a transaction", recipientPublicAccount);
const transferTransaction = TransferTransaction.create(
    TimeWindow.createWithDeadline(),
    recipientPublicAccount.address,
    new XEM(2),
    encryptedMessage
);


```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/transaction/How_to_create_a_Transfer_Transaction_with_an_Encrypted_Message.ts)

### How to create a Transfer Transaction with Mosaics

```typescript
/**
 * nem-library 0.5.1
 */
import {
    NEMLibrary, NetworkTypes, Address, TransferTransaction, TimeWindow,
    MosaicHttp, TransactionHttp, Account, EmptyMessage
} from "nem-library";
import {Observable} from "rxjs/Observable";

declare let process: any;

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

// Replace with a cosignatory private key
const privateKey: string = process.env.PRIVATE_KEY;

const transactionHttp = new TransactionHttp({domain: "104.128.226.60"});
const mosaicHttp = new MosaicHttp({domain: "104.128.226.60"});
const account = Account.createWithPrivateKey(privateKey);

Observable.from([
    {namespace: "mynamespace", mosaic: "mosaic1", quantity: 10},
    {namespace: "mynamespace", mosaic: "mosaic2", quantity: 10},
    {namespace: "mynamespace", mosaic: "mosaic3", quantity: 10}
]).flatMap(_ => mosaicHttp.getMosaicTransferableWithAmount(_.namespace, _.mosaic, _.quantity))
    .toArray()
    .map(mosaics => TransferTransaction.createWithMosaics(
        TimeWindow.createWithDeadline(),
        new Address("TBV7LE4TFDEMGVOON5MYOK2P7TU2KEKLMHOLHQT6"),
        mosaics,
        EmptyMessage
        )
    )
    .map(transaction => account.signTransaction(transaction))
    .flatMap(signedTransaction => transactionHttp.announceTransaction(signedTransaction))
    .subscribe(nemAnnounceResult => {
        console.log(nemAnnounceResult);
    })

```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/transaction/How_to_create_a_Transfer_Transaction_with_Mosaics.ts)



### How to create a MultiSig Transaction

```typescript
import {
    NEMLibrary, NetworkTypes, Address, TransferTransaction, TimeWindow,
    EmptyMessage, MultisigTransaction, PublicAccount, TransactionHttp, Transaction, XEM
} from "nem-library";

declare let process: any;

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

// Replace with the multisig account
const multisigAccountPublicKey: string = process.env.MULTISIG_PUBLIC_KEY;

const transferTransaction: Transaction = TransferTransaction.create(
    TimeWindow.createWithDeadline(),
    new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"),
    new XEM(2),
    EmptyMessage
);

const multisigTransaction: MultisigTransaction = MultisigTransaction.create(
    TimeWindow.createWithDeadline(),
    transferTransaction,
    PublicAccount.createWithPublicKey(multisigAccountPublicKey)
);
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/transaction/How_to_create_a_MultiSig_Transaction.ts)

### How to filter Transactions by type

```typescript
import {
    AccountHttp, Address, MultisigTransaction, NEMLibrary, NetworkTypes, Transaction,
    TransactionTypes
} from "nem-library";

NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const accountHttp = new AccountHttp();

accountHttp.allTransactions(new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"))
    .map((transactions: Transaction[]): MultisigTransaction[] => {
        console.log(">>>>>>>>>>>>");
        console.log("All Transactions", transactions);
        return <MultisigTransaction[]>transactions.filter(x => x.type == TransactionTypes.MULTISIG)
    })
    .subscribe((x: MultisigTransaction[]) => {
        console.log("\n\n>>>>>>>>>>>>");
        console.log("Just Multisig", x)
    });
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/transaction/How_to_filter_Transactions_by_type.ts)