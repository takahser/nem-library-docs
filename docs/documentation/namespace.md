# Namespace related requests

NEM supports the concept of namespaces which is the NEM analog of internet domain names. A namespace is an identification string that consists of one or more parts that are concatenated by dots, for example 'makoto.metals.silver'. All namespaces are unique and thus can only have one owner at a time. A namespace that has only one part is called a root namespace, otherwise sub-namespace. Root namespaces can be rented by accounts for the duration of one year. One month before the root namespace expires the rental contract can be renewed for another year. If a root namespace rental contract is renewed, all sub-namespaces are valid for another year as well. If the root namespace is not renewed, it exires together with all sub-namespaces. One month after a root namespace expires, another account is able to rent that root namespace. The new owner does not inherit the sub-namespaces from the previous owner however. An account can only rent a sub-namespace if it owns the corresponding root namespace.

Namespaces have certain restrictions with respected to the characters being allowed in the parts as well as the length of a part. A root namespace may have a length of 16 characters while sub-namespaces may have a length of 64 characters.

[Official Source](https://bob.nem.ninja/docs/#namespaces)

# NamespaceHttp definition

```typescript
export declare class NamespaceHttp extends HttpEndpoint {
    constructor(nodes?: ServerConfig[]);
    
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

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const namespaceHttp = new NamespaceHttp();
const id = 12344;

namespaceHttp.getRootNamespaces(id).subscribe(namespaces => {
    console.log(namespaces);
});
```

Output

```
[ Namespace {
    name: 'ajkshgkerhkjerg',
    owner:
     Address {
       value: 'TDQJM2LZPKM4WRI44CF5TEUHRES5JCWQ3YRKGKUB',
       networkType: 152 },
    height: 1034366,
    id: 409 },
  Namespace {
    name: 'hitori',
    owner:
     Address {
       value: 'TCIY4MHDHZJ42NGHQ67PJXSWHLFACNMJLSM56VE3',
       networkType: 152 },
    height: 1033761,
    id: 408 },
  Namespace {
    name: 'vandelay',
    owner:
     Address {
       value: 'TAB5AEUDM3ALLDRLRLNMQYQXJXJZUSMDSEN5TND5',
       networkType: 152 },
    height: 1032986,
    id: 405 },
  Namespace {
    name: 'girish',
    owner:
     Address {
       value: 'TAFHY34ZKCJIYI6LPEJXVOCMVK5QHQXJ5AAPO5RG',
       networkType: 152 },
    height: 1031091,
    id: 403 },
  Namespace {
    name: 'multisigns',
    owner:
     Address {
       value: 'TDAEIWWYWZRE3VSUHXG77L5TW22BHNGV5FAPH2XJ',
       networkType: 152 },
    height: 1029552,
    id: 402 },
  Namespace {
    name: 'foobarltd',
    owner:
     Address {
       value: 'TANXPOUPPCJHL3PLUHY5M6AE6B47H3YZ5ZCDFMXH',
       networkType: 152 },
    height: 1029471,
    id: 401 },
  Namespace {
    name: 'jabo',
    owner:
     Address {
       value: 'TANXPOUPPCJHL3PLUHY5M6AE6B47H3YZ5ZCDFMXH',
       networkType: 152 },
    height: 1029471,
    id: 400 },
  Namespace {
    name: 'newpart',
    owner:
     Address {
       value: 'TCJZJHAV63RE2JSKN27DFIHZRXIHAI736WXEOJGA',
       networkType: 152 },
    height: 1028225,
    id: 399 },
  Namespace {
    name: 'govegan',
    owner:
     Address {
       value: 'TBXWZW35ZJDHLXYDQ6EQTWKHJSWQG2EQYWDRKXXU',
       networkType: 152 },
    height: 1027004,
    id: 398 },
  Namespace {
    name: 'tescik11',
    owner:
     Address {
       value: 'TCWHXK5EI3RJIUV5DXURIIEASECB57L4N5EEGI24',
       networkType: 152 },
    height: 1026918,
    id: 397 },
  Namespace {
    name: 'fintech',
    owner:
     Address {
       value: 'TBN24GPJMXTMI3VRFRDKWL6A2FIM2YGHMHD4OPUT',
       networkType: 152 },
    height: 1026764,
    id: 396 },
  Namespace {
    name: 'kung',
    owner:
     Address {
       value: 'TAU5HO3DRQZNELFEMZZTUKQEZGQ7IUAHKPO7OOLK',
       networkType: 152 },
    height: 1025402,
    id: 395 },
  Namespace {
    name: 'dim',
    owner:
     Address {
       value: 'TDXTBQUI5PCPHBZKTHBSLMCJGKDWV3RVHHOCPX2V',
       networkType: 152 },
    height: 1022794,
    id: 393 },
  Namespace {
    name: 'multisig',
    owner:
     Address {
       value: 'TAVBSWJV3XNA7MRVEH3XQQYRTJGXM3VRKCQDKHR7',
       networkType: 152 },
    height: 1021094,
    id: 392 },
  Namespace {
    name: 'jeffmcdonald',
    owner:
     Address {
       value: 'TBNDYR4AVGYFEEUQ5LBPNEON42HSQ37NYGLZC344',
       networkType: 152 },
    height: 1019783,
    id: 391 },
  Namespace {
    name: 'lonwon',
    owner:
     Address {
       value: 'TBNDYR4AVGYFEEUQ5LBPNEON42HSQ37NYGLZC344',
       networkType: 152 },
    height: 1019768,
    id: 390 },
  Namespace {
    name: 'bok_tor',
    owner:
     Address {
       value: 'TBN24GPJMXTMI3VRFRDKWL6A2FIM2YGHMHD4OPUT',
       networkType: 152 },
    height: 1018248,
    id: 389 },
  Namespace {
    name: 'macy',
    owner:
     Address {
       value: 'TCF2JGBTB7LM6Y3VP22DSSFRUGDYZYVQMUGDX4TL',
       networkType: 152 },
    height: 1018245,
    id: 388 },
  Namespace {
    name: 'foo',
    owner:
     Address {
       value: 'TAOMBDIWECBSP7QGEKGUE476QOCRRN6MEOSQQ4RP',
       networkType: 152 },
    height: 1017249,
    id: 387 },
  Namespace {
    name: 'namespace_root1',
    owner:
     Address {
       value: 'TDUI4FXZDEMSFMGM7BOQOLENGX7P7Q6I7ISIH7NK',
       networkType: 152 },
    height: 1017028,
    id: 385 },
  Namespace {
    name: 'root_namespace1',
    owner:
     Address {
       value: 'TDUI4FXZDEMSFMGM7BOQOLENGX7P7Q6I7ISIH7NK',
       networkType: 152 },
    height: 1017028,
    id: 384 },
  Namespace {
    name: 'rivalkingdoms',
    owner:
     Address {
       value: 'TCBAR5KG4HHTF3EBBCQETVYUJVONF7XKWSYCI2CA',
       networkType: 152 },
    height: 1016967,
    id: 382 },
  Namespace {
    name: 'szpregel',
    owner:
     Address {
       value: 'TDFPX267BPLVUB2VF3OY2LSACGSF4XWLVPUXY3FC',
       networkType: 152 },
    height: 1016780,
    id: 381 },
  Namespace {
    name: 'marvel',
    owner:
     Address {
       value: 'TDSPQOUYI6VBGD2SAERJ73ZYMNY5ACJSYNTZSUHP',
       networkType: 152 },
    height: 1016477,
    id: 379 },
  Namespace {
    name: 'test',
    owner:
     Address {
       value: 'TADEADUYY4FE5JSHL22VNASNYXYXEJVZ7BF2E3IO',
       networkType: 152 },
    height: 1015685,
    id: 378 } ]
```

[Run the code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/concepts/namespace/NamespaceHttpExample.ts)

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