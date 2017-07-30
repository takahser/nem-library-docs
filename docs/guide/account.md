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

### <a name='alltransactionspaginated'></a> How to receive all transactions for an account paginated

```typescript
/**
 * nem-library 0.3.0
 */

import { AccountHttp, Address, NEMLibrary, NetworkTypes } from "nem-library";

NEMLibrary.bootstrap(NetworkTypes.TEST_NET);
let address = new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J");
let accountHttp = new AccountHttp();

let pageable = accountHttp.allTransactionsPaginated(address);

pageable.subscribe(transactions => {
    // do something with the info
});

pageable.nextPage(); // Fetch the nexts 25 transactions
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/account/How_to_do_pagination_in_transactions_methods.ts)



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

### How to get Account harvested blocks info paginated

```typescript
import {AccountHttp, Address, NEMLibrary, NetworkTypes} from "nem-library";

NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const accountHttp = new AccountHttp();

accountHttp.getAccountImportances()
    .subscribe(x => {
        console.log(x);
    });
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/account/How_to_do_pagination_in_account_harvests.ts)

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

// Initialize NEMLibrary for TEST_NET Network
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

### How to enable harvesting

```typescript
import {
    NEMLibrary, NetworkTypes, Account, TimeWindow,
    PublicAccount, TransactionHttp, ImportanceMode, ImportanceTransferTransaction, AccountHttp
} from "nem-library";
declare let process: any;

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const privateKey: string = process.env.PRIVATE_KEY;
const delegateAccountHarvestingPrivateKey: string = process.env.HARVESTING_PRIVATE_KEY;

const account = Account.createWithPrivateKey(privateKey);
const delegatedAccount = Account.createWithPublicKey(delegateAccountHarvestingPrivateKey);

const importanceTransferTransaction = ImportanceTransferTransaction.create(
    TimeWindow.createWithDeadline(),
    ImportanceMode.Activate,
    PublicAccount.createWithPublicKey(delegatedAccount.publicKey)
);

const signedTransaction = account.signTransaction(importanceTransferTransaction);

const transactionHttp = new TransactionHttp({domain: "104.128.226.60"});
transactionHttp.announceTransaction(signedTransaction).subscribe(x => console.log(x));

// Wait aproximately 6h for you delegated account to be active

const accountHttp = new AccountHttp({domain: "104.128.226.60"});

// Testnet supernode
const supernodeDomain = "188.68.50.161";

accountHttp.unlockHarvesting(supernodeDomain, delegateAccountHarvestingPrivateKey).subscribe(success => console.log(success));

```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/account/How_to_enable_harvesting.ts)

### How receive all transactions

The sample shows how to fetch all the transactions for an account, emulating a loop or a recursive function.

```typescript
import {AccountHttp, Address, NEMLibrary, NetworkTypes} from "nem-library";

NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const accountHttp = new AccountHttp();
const address = new Address("TCFFOM-Q2SBX7-7E2FZC-3VX43Z-TRV4ZN-TXTCGW-BM5J");
let pagedTransactions = accountHttp.allTransactionsPaginated(address, undefined, 100);
let time = 0;
pagedTransactions.subscribe(x => {
    console.log("TIME ", ++time);
    console.log("transactions size", x.length);
    // Fetch the next 100 transactions
    pagedTransactions.nextPage();
}, err => {
    console.log("error");
}, () => {
    // when this lambda is called, it means all transactions have been fetched
    console.log("complete");
});
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/account/How_to_fetch_all_transactions_for_an_account.ts)
