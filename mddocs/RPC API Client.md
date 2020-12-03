# RPC API Client

## Creating client

Refer to [Quickstart](Quickstart.md) for creating clients.

## RPC APIs

Tronj wraps RPC APIs defined in Google Protobuf, makes the use of the interfaces  become convinient.

The APIs can be simply divided into two types: *system contract* and *smart contract*.

Tron network has two types of resources: *bandwidth* and *energy*. *System contracts* consume only bandwidth and *Smart contracts* may need both(only trigger calls).

### System Contract

System contract is one feature of TRON network.  

A Transaction equals to a system contract call, there are two types of APIs: *Transaction API* and *Query API*.

#### Send a transaction APIs
The routine of sending transactions is: [Sending Transaction](Sending Transaction.md).

**# transfer(String from, String to, long amount)**

Transfer TRX. amount in SUN

```java
public TransactionReturn transfer(String from, String to, long amount) {

        ByteString rawFrom = parseAddress(from);
        ByteString rawTo = parseAddress(to);

        TransferContract req = TransferContract.newBuilder()
                .setOwnerAddress(rawFrom)
                .setToAddress(rawTo)
                .setAmount(amount)
                .build();

        TransactionExtention txnExt = blockingStub.createTransaction2(req);

        Transaction signedTxn = signTransaction(txnExt);

        TransactionReturn ret = blockingStub.broadcastTransaction(signedTxn);
        
        return ret;
    }
```

#### Query APIs
The Tron wraps many query APIs and utility functions. You can query the chain using a instance.

**# getNowBlock()**

Get the latest block
  
```java
public void getNowBlock() {
        System.out.println("============= getNowBlock =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            client.getNowBlock();
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
}
```

### Smart Contract

There are two types of smart contract calls: constant and trigger. Refer to [Smart Contract](Smart Contract.md).



