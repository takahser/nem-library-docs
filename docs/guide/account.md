### How to receive all transactions for an account

```typescript
import {AccountHttp, Address, NEMLibrary, NetworkTypes} from "nem-library";

NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const accountHttp = new AccountHttp();

accountHttp.allTransactions(new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"))
    .subscribe(allTransactions => {
       console.log(allTransactions);
    });
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/account/How_to_receive_the_transactions_for_an_account.ts)

### How to receive incoming transactions for an account 

```typescript
import {AccountHttp, Address, NEMLibrary, NetworkTypes} from "nem-library";

NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const accountHttp = new AccountHttp();

accountHttp.incomingTransactions(new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"))
    .subscribe(x => {
        console.log(x);
    });
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/account/How_to_receive_incoming_transactions_for_an_account.ts)

### How to receive outgoing transactions for an account

```typescript
import {AccountHttp, Address, NEMLibrary, NetworkTypes} from "nem-library";

NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const accountHttp = new AccountHttp();

accountHttp.outgoingTransactions(new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"))
    .subscribe(x => {
        console.log(x);
    });
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/account/How_to_receive_outgoing_transactions_for_an_account.ts)

### How to get the unconfirmed transactions for an account 

```typescript
import {AccountHttp, Address, NEMLibrary, NetworkTypes} from "nem-library";

NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const accountHttp = new AccountHttp();

accountHttp.unconfirmedTransactions(new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J"))
    .subscribe(x => {
        console.log(x);
    });
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/account/How_to_get_the_unconfirmed_transactions_for_an_account.ts)

### How to get Account Importances

```typescript
import {AccountHttp, Address, NEMLibrary, NetworkTypes} from "nem-library";

NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const accountHttp = new AccountHttp();

accountHttp.getAccountImportances()
    .subscribe(x => {
        console.log(x);
    });
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/account/How_to_get_Account_Importances.ts)

### How to convert a Normal Account into Multisig Account

```typescript
import {
    AccountHttp, NEMLibrary, NetworkTypes, Address, Account, TransferTransaction, TimeWindow,
    EmptyMessage, MultisigTransaction, PublicAccount, TransactionHttp, XEM, MultisigAggregateModificationTransaction,
    CosignatoryModification, CosignatoryModificationAction
} from "nem-library";
declare let process: any;

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const transactionHttp = new TransactionHttp({domain: "104.128.226.60"});

// Replace with the private key of the account that you want to convert into multisig
const privateKey: string = process.env.PRIVATE_KEY;
const cosignatory1PublicKey: string = process.env.COSIGNATORY_1_PUBLIC_KEY;
const cosignatory2PublicKey: string = process.env.COSIGNATORY_2_PUBLIC_KEY;

const account = Account.createWithPrivateKey(privateKey);

const cosignatory1 = PublicAccount.createWithPublicKey(cosignatory1PublicKey);
const cosignatory2 = PublicAccount.createWithPublicKey(cosignatory2PublicKey);

const convertIntoMultisigTransaction = MultisigAggregateModificationTransaction.create(
    TimeWindow.createWithDeadline(),
    [
        new CosignatoryModification(cosignatory1, CosignatoryModificationAction.ADD),
        new CosignatoryModification(cosignatory2, CosignatoryModificationAction.ADD),
    ],
    2
);

const signedTransaction = account.signTransaction(convertIntoMultisigTransaction);

transactionHttp.announceTransaction(signedTransaction).subscribe(x => console.log(x));
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/account/How_to_convert_a_Normal_Account_into_Multisig_Account.ts)