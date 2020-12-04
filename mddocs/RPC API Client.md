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

**transfer(String fromAddress, String toAddress, long amount)**

Transfer TRX. amount in SUN

```java
public TransactionExtention transfer(String fromAddress, String toAddress, long amount) throws IllegalException {

        ByteString rawFrom = parseAddress(fromAddress);
        ByteString rawTo = parseAddress(toAddress);

        TransferContract req = TransferContract.newBuilder()
                .setOwnerAddress(rawFrom)
                .setToAddress(rawTo)
                .setAmount(amount)
                .build();
        TransactionExtention txnExt = blockingStub.createTransaction2(req);

        if(SUCCESS != txnExt.getResult().getCode()){
            throw new IllegalException(txnExt.getResult().getMessage().toStringUtf8());
        }

        return txnExt;
    }
```

#### Query APIs
Tronj wraps query APIs. With a TronClient instance, you can call the APIs simply like below:
**Note:** Query APIs don't require signature and broadcasting. You may call query APIs with any hex String conforming to the private key format.

**getNowBlock()**

Get the latest block
  
```java
public Block getNowBlock() throws IllegalException {
        Block block = blockingStub.getNowBlock(EmptyMessage.newBuilder().build());
        if(!block.hasBlockHeader()){
            throw new IllegalException("Fail to get latest block.");
        }
        return block;
    }
```

### Smart Contract

There are two types of smart contract calls: constant and trigger. Smart contract operations are different from system contracts. Refer to [Smart Contract](Smart Contract.md).



