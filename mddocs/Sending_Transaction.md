# Sending Transaction

Any operation contracting with TRON network is a transaction. A transaction can be TRX transfer, TRC-10 transfer, freezing & unfreezing, voting, Etc. 

## The routine for sending

A normal routine for sending a transaction is:

> Create -> Sign -> Broadcast -> (wait) -> Lookup and get receipt

Tronj defines the same protobufs include all RPC APIs and transaction related classes, also, Tronj wraps the functions in `TronClient.class`.

### An simple example

```java
public void sendTrx() {
        System.out.println("============= TRC transfer =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            TransactionExtention transactionExtention = client.transfer("owner address", "to address", 1_000_000);
            Transaction signedTxn = client.signTransaction(transactionExtention);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

In Tronj, the routine is:

1. Create a `TronClient` object with your private key.
2. Obtain a `TransactionExtention` object from `TronClient.transfer()`.
3. Call `TronClient.signTransaction()`to sign the transaction with your private key binding with the `TronClient` object.
4. Call `TronClient.broadcastTransaction()` to broadcast the transaction and get a `TransactionReturn` for analysis.

