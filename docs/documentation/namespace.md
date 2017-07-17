# Namespace related requests

NEM supports the concept of namespaces which is the NEM analog of internet domain names. A namespace is an identification string that consists of one or more parts that are concatenated by dots, for example 'makoto.metals.silver'. All namespaces are unique and thus can only have one owner at a time. A namespace that has only one part is called a root namespace, otherwise sub-namespace. Root namespaces can be rented by accounts for the duration of one year. One month before the root namespace expires the rental contract can be renewed for another year. If a root namespace rental contract is renewed, all sub-namespaces are valid for another year as well. If the root namespace is not renewed, it exires together with all sub-namespaces. One month after a root namespace expires, another account is able to rent that root namespace. The new owner does not inherit the sub-namespaces from the previous owner however. An account can only rent a sub-namespace if it owns the corresponding root namespace.

Namespaces have certain restrictions with respected to the characters being allowed in the parts as well as the length of a part. A root namespace may have a length of 16 characters while sub-namespaces may have a length of 64 characters.

[Official Source](https://bob.nem.ninja/docs/#namespaces)

# NamespaceHttp definition

```typescript
export declare class NamespaceHttp extends HttpEndpoint {
    constructor(serverConfig?: ServerConfig);
    
    /**
     * Gets the root namespaces. The requests supports paging, i.e. retrieving the root namespaces in batches of a specified size.
     * @param id - The topmost namespace database id up to which root namespaces are returned. The parameter is optional. If not supplied the most recent rented root namespaces are returned.
     * @param pageSize - (Optional) The number of namespace objects to be returned for each request. The parameter is optional. The default value is 25, the minimum value is 5 and hte maximum value is 100.
     * @returns Observable<Namespace[]>
     */
    getRootNamespaces(id: number, pageSize?: string): Observable<Namespace[]>;
    
    /**
     * Gets the namespace with given id.
     * @param namespace - The namespace id.
     * @returns Observable<Namespace>
     */
    getNamespace(namespace: string): Observable<Namespace>;
}

```

# NamespaceHttp usage

```typescript
import {NamespaceHttp, NEMLibrary, NetworkTypes} from "nem-library";

// Inicializate NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const namespaceHttp = new NamespaceHttp({domain: "104.128.226.60"});
const id = 12344;

namespaceHttp.getRootNamespaces(id).subscribe(namespaces => {
    console.log(namespaces);
});
```

Output

# Models

## Namespace

```typescript
/**
 * A namespace is the NEM version of a domain. You can rent a namespace for the duration of a year by paying a fee.
 * The naming of the parts of a namespace has certain restrictions, see the corresponding chapter on namespaces.
 */
export declare class Namespace {
    /**
     * The fully qualified name of the namespace, also named namespace id.
     */
    readonly name: string;
    
    /**
     * The owner of the namespace.
     */
    readonly owner: Address;
    
    /**
     * The height at which the ownership begins.
     */
    readonly height: number;
    
    /**
     * The database id for the namespace object.
     */
    readonly id?: number;
    
}
```