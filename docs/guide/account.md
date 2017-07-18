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

### How to convert a Normal Account into Multisig Account

```typescript

```