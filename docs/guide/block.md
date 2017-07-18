# Block

### How to get a Block by Height

```typescript
import {BlockHttp, NEMLibrary, NetworkTypes} from "nem-library";

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const blockHttp = new BlockHttp({domain: "104.128.226.60"});
blockHttp.getBlockByHeight(1033023).subscribe(block => {
    console.log(block);
});
```