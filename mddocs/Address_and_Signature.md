# Address and Signature

Tronj supports offline private key generation and signature.

## Address Generation

The `generateAddress` function can be directly called without binding a private key.

```java
TronClient.generateAddress();
```

This returns a private key that can be directly used on TRON network.

## Signature

Tronj has encapsulated signature function, refer to `signTransaction` in [RPC_APIs](RPC_APIs.md).

Binding a private key with a TronClient instance or providing a `SECP256K1.KeyPair` to accomplish the signing process.