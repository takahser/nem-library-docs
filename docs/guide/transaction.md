### How to create a Transfer Transaction

```typescript
import {
    NEMLibrary, NetworkTypes, Address, TransferTransaction, Transaction, TimeWindow,
    EmptyMessage, MultisigTransaction, PublicAccount, TransactionHttp
} from "nem-library";
import {XEM} from "nem-library/dist/src/models/mosaic/XEM";

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const transferTransaction: Transaction = TransferTransaction.create(
    TimeWindow.createWithDeadline(),
    new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"),
    XEM(2),
    EmptyMessage
);
```

### How to sign a Transaction

```typescript
import {
    AccountHttp, NEMLibrary, NetworkTypes, Address, Account, TransferTransaction, TimeWindow,
    EmptyMessage, MultisigTransaction, PublicAccount, TransactionHttp
} from "nem-library";
import {XEM} from "nem-library/dist/src/models/mosaic/XEM";
declare let process: any;

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const transactionHttp = new TransactionHttp({domain: "104.128.226.60"});

// Replace with a cosignatory private key
const privateKey: string = process.env.PRIVATE_KEY;

const account = Account.createWithPrivateKey(privateKey);

const transferTransaction = TransferTransaction.create(
    TimeWindow.createWithDeadline(),
    new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"),
    XEM(2),
    EmptyMessage
);

const signedTransaction = account.signTransaction(multisigTransaction);

transactionHttp.announceTransaction(signedTransaction).subscribe( x => console.log(x));
```

### How to send a Transaction with a Message

```typescript
import {
    NEMLibrary, NetworkTypes, Address, TransferTransaction, Transaction, TimeWindow,
    EmptyMessage, MultisigTransaction, PublicAccount, TransactionHttp, Message
} from "nem-library";
import {XEM} from "nem-library/dist/src/models/mosaic/XEM";

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const transferTransaction: Transaction = TransferTransaction.create(
    TimeWindow.createWithDeadline(),
    new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"),
    XEM(0),
    Message.create("a transaction")
);
```

### How to create a MultiSig Transaction

```typescript
import {
    NEMLibrary, NetworkTypes, Address, TransferTransaction, TimeWindow,
    EmptyMessage, MultisigTransaction, PublicAccount, TransactionHttp, Transaction
} from "nem-library";
import {XEM} from "nem-library/dist/src/models/mosaic/XEM";

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

// Replace with the multisig account
const multisigAccountPublicKey: string = process.env.MULTISIG_PUBLIC_KEY;

const transferTransaction: Transaction = TransferTransaction.create(
    TimeWindow.createWithDeadline(),
    new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"),
    XEM(2),
    EmptyMessage
);

const multisigTransaction: MultisigTransaction = MultisigTransaction.create(
    TimeWindow.createWithDeadline(),
    transferTransaction,
    PublicAccount.createWithPublicKey(multisigAccountPublicKey)
);
```