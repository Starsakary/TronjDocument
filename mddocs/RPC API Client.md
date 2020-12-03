# RPC API Client

## Creating client

Refer to [Quickstart](Quickstart.md) for creating clients.

## RPC APIs

Tronj wraps RPC APIs defined in Google Protobuf, makes the use of the interfaces  become convinient.

The APIs can be simply divided into two types: *system contract* and *smart contract*.

Tron network has two types of resources: *bandwidth* and *energy*. *System contracts* consume only bandwidth and *Smart contracts* may need both(only trigger calls).

### System Contract. 

System contract is one feature of TRON network.  

A Transaction in TRON is a system contract call, the TronClient(transaction) APIs include two types: *Send a transaction APIs* and *Query APIs*.

#### Send a transaction APIs
The routine for sending refers to [Sending Transaction](https://github.com/Starsakary/TronjDocs/blob/develop/source/mddocs/Sending%20Transaction.md).

**# transfer(String fromAddress, String toAddress, long amount)**

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
The Tron wraps many query APIs and utility functions. You can query the chain using a instance.

**# getNowBlock()**

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

There are two types of smart contract calls: constant and trigger. Refer to [Smart Contract](https://github.com/Starsakary/TronjDocs/blob/develop/source/mddocs/Smart%20Contract.md).



