# Blockchain

### How to get a Block by Height

```typescript
import {BlockHttp, NEMLibrary, NetworkTypes} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const blockHttp = new BlockHttp();
blockHttp.getBlockByHeight(1033023).subscribe(block => {
    console.log(block);
});
```
[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/blockchain/How_to_get_a_Block_by_Height.ts)


### How to get Last block of the blockchain

```typescript
import {ChainHttp, NEMLibrary, NetworkTypes} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const chainHttp = new ChainHttp();
chainHttp.getBlockchainLastBlock().subscribe(block => {
    console.log(block);
});
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/blockchain/How_to_get_Last_block_of_the_blockchain.ts)




