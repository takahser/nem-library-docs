### How to create a Namespace

```typescript
import{
    NEMLibrary, NetworkTypes, Transaction, TimeWindow, ProvisionNamespaceTransaction, Account, TransactionHttp
} from "nem-library";

declare let process: any;

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const privateKey: string = process.env.PRIVATE_KEY;
const account = Account.createWithPrivateKey(privateKey);
const transactionHttp = new TransactionHttp();

const namespace = "new-namespace";

const provisionNamespaceTransaction: Transaction = ProvisionNamespaceTransaction.create(
    TimeWindow.createWithDeadline(),
    namespace
);

const signedTransaction = account.signTransaction(provisionNamespaceTransaction);
transactionHttp.announceTransaction(signedTransaction).subscribe( x => console.log(x));
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/namespace/How_to_create_a_Namespace.ts)

### How to create a Sub-Namespace

```typescript
import {
    NEMLibrary, NetworkTypes, Transaction, TimeWindow, ProvisionNamespaceTransaction, Account, TransactionHttp
} from "nem-library";

declare let process: any;

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const privateKey: string = process.env.PRIVATE_KEY;
const account = Account.createWithPrivateKey(privateKey);
const transactionHttp = new TransactionHttp();

const namespace = "new-namespace";
const subnamespace = "subnamespace";

const provisionNamespaceTransaction: Transaction = ProvisionNamespaceTransaction.create(
    TimeWindow.createWithDeadline(),
    subnamespace,
    namespace
);

const signedTransaction = account.signTransaction(provisionNamespaceTransaction);
transactionHttp.announceTransaction(signedTransaction).subscribe( x => console.log(x));

```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/namespace/How_to_create_a_Sub_Namespace.ts)

### How to know if a Namespace exists

```typescript
import {
    NEMLibrary, NetworkTypes, NamespaceHttp
} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const namespaceHttp = new NamespaceHttp();
const namespace = "new-namespace";

namespaceHttp.getNamespace(namespace).subscribe(namespace => console.log(namespace));
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/namespace/How_to_know_if_a_Namespace_exists.ts)


### How to know the owner of a Namespace

```typescript
import {
    NEMLibrary, NetworkTypes, NamespaceHttp
} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const namespaceHttp = new NamespaceHttp();
const namespace = "new-namespace";

namespaceHttp.getNamespace(namespace).subscribe(namespace => console.log(namespace));

```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/namespace/How_to_know_the_owner_of_a_Namespace.ts)
