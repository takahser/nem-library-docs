# Custom Mosaics Definitions

NEM Library adds already defined [Mosaics](mosaic.md) to be used like [XEM](mosaic.md#XEM) model.

### How to request a new Mosaic Definition

We like to support as Mosaics as we can, just [email us](../about.md) with the mosaic that you would like to add to NEM Library. 

We just require that the mosaic **must** not be mutable.

# DimCoin Model

DimToken is from [DIMCOIN](https://www.dimcoin.io/)

```typescript
export class DimCoin extends MosaicTransferable {

  constructor(amount: number) {
    super(new MosaicId("dim", "coin"), new MosaicProperties(6, 9000000000, true, false), amount);
  }
}
```

# DimToken Model

DimToken is from [DIMCOIN](https://www.dimcoin.io/)

```typescript
export class DimToken extends MosaicTransferable {

  constructor(amount: number) {
    super(new MosaicId("dim", "token"), new MosaicProperties(6, 10000000, true, false), amount);
  }
}
```

# EcobitEco Model

EcobitEco is from [Ecobit project](http://www.ecobit.io/)

```typescript
export class EcobitEco extends MosaicTransferable {
  constructor(amount: number) {
    super(new MosaicId("ecobit", "eco"), new MosaicProperties(0, 8888888888, true, false), amount);
  }
}
```