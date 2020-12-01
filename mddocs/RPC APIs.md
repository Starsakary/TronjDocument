# RPC APIs  

This chapter describes the specific definitions, parameters, return values and exception handling of the RPC APIs.

## Full Node APIs   

### signTransaction

Sign a transactionExtention with the client binding private key.

**PARAMS**

*1. txnExt(TransactionExtention)*

A TransactionExtention object.

**RETURN**

A signed transaction.

### signTransaction

Sign a transaction with the client binding private key.

**PARAMS**

*1. txn(Transaction)*

A Transaction object.

**RETURN**

A signed transaction.

### signTransaction

Sign a transaction with a private key.

**PARAMS**

*1. txn(Transaction)*

A Transaction object.

*2. kp(SECP256K1.KeyPair)*

The private key to sign the transaction.

**RETURN**

A signed transaction.

### signTransaction

Sign a transactionExtention with a private key.

**PARAMS**

*1. txnExt(TransactionExtention)*

A TransactionExtention object.

*2. kp(SECP256K1.KeyPair)*

The private key to sign the transaction.

**RETURN**

A signed transaction.

### generateAddress  

Generate random address.

**RETURN**  

Address in hex.

### Transfer

Transfer TRX. amount in SUN. 

**PARAMS**  

*1. fromAddress(String)**  

fromAddress is the owner address.

*2. toAddress(String)**  

toAddress is the recipient address.  

*3. amount(int)**  

amount is the amount of TRX to transfer in SUN.  

**RETURN**  

Transaction, including execution results.  

**THROWS**  

IllegalException, if fail to transfer.

### TransferTrc10

Transfers TRC10 Asset  

**PARAMS**  

*1. fromAddress(String)**  

fromAddress is the owner address.  

*2. toAddress(String)**  

toAddress is the recipient address.  

*3. tokenId(int)**  

asset name

*4. amount(int)**  

amount is the amount of TRX to transfer in SUN.  

**RETURN**  

Transaction, including execution results.  

**THROWS**  

IllegalException, if fail to transfer trc10.

### freezeBalance

Freeze balance to get energy or bandwidth, for 3 days.  

**PARAMS**  

*1. ownerAddress(String)**  

owner address, default hexString.  

*2. frozenBalance(long)**  

TRX freeze amount, the unit is sun.

*3. frozenDuration(long)**  

TRX freeze duration, only be specified as 3 days.

*4. resourceCode(int)**  

resource type, can be "ENERGY" or "BANDWIDTH"

```java
public TransactionReturn freezeBalance(String ownerAddress, long frozenBalance, long frozenDuration, int resourceCode) throws IllegalException{

        ByteString rawFrom = parseAddress(ownerAddress);
        FreezeBalanceContract freezeBalanceContract=
                FreezeBalanceContract.newBuilder()
                        .setOwnerAddress(rawFrom)
                        .setFrozenBalance(frozenBalance)
                        .setFrozenDuration(frozenDuration)
                        .setResourceValue(resourceCode)
                        .build();
        TransactionExtention txnExt = blockingStub.freezeBalance2(freezeBalanceContract);

        if(SUCCESS != txnExt.getResult().getCode()){
            throw new IllegalException(txnExt.getResult().getMessage().toStringUtf8());
        }

        Transaction signedTxn = signTransaction(txnExt);

        TransactionReturn ret = blockingStub.broadcastTransaction(signedTxn);
        return ret;
    }
```

**RETURN**  

Transaction, including execution results.  

**THROWS**  

IllegalException, if fail to freeze balance.

### unfreezeBalance 

Unfreeze balance to get TRX back.  

**PARAMS**  

*1. ownerAddress(String)**  

owner address, default hexString.  

*2. resourceCode(int)**  

resource type, can be "ENERGY" or "BANDWIDTH"  

**RETURN**  

Transaction, including execution results.  

**THROWS**  

IllegalException, if fail to unfreeze balance.

### voteWitness 

Vote for witnesses  

**PARAMS**  

*1. ownerAddress(String)**  

owner address, default hexString.  

*2. votes(Map)**  

key: 'vote_address' stands for the address of the witness you want to vote, default hexString.  

value: 'vote_count' stands for the number of votes you want to vote.

```java
public TransactionReturn voteWitness(String ownerAddress, HashMap<String, String> votes) throws IllegalException{
        ByteString rawFrom = parseAddress(ownerAddress);
        VoteWitnessContract voteWitnessContract = createVoteWitnessContract(rawFrom, votes);
        TransactionExtention txnExt = blockingStub.voteWitnessAccount2(voteWitnessContract);

        if(SUCCESS != txnExt.getResult().getCode()){
            throw new IllegalException(txnExt.getResult().getMessage().toStringUtf8());
        }

        Transaction signedTxn = signTransaction(txnExt);

        TransactionReturn ret = blockingStub.broadcastTransaction(signedTxn);

        return ret;
    }
```  
**RETURN**  

Transaction, including execution results.  

**THROWS**  

IllegalException, if fail to vote witness.

### getNowBlock

Query the latest block information. 

**RETURN**  

Block object.  

**THROWS**  

IllegalException, if fail to get now block.

### getBlockByNum  

Returns the Block Object corresponding to the 'Block Height' specified (number of blocks preceding it).  

**PARAMS**  

*1. blockNum(long)**  

blockNum is the block height.  

**RETURN**  

Block object.  

**THROWS**  

IllegalException, if the parameters are not correct.

### getBlockByLatestNum 

Get some latest blocks.  

**PARAMS**  

*1. num(long)**  

Number of latest blocks.  

**RETURN**  

BlockListExtention object.  

**THROWS**  

IllegalException, if the parameters are not correct.

### getNodeInfo

Get current API node’ info.  

**RETURN**  

NodeInfo object.  

**THROWS**  

IllegalException, if fail to get nodeInfo.

### listNodes

List all nodes that current API node is connected to.  

**RETURN**  

NodeList object.  

**THROWS**  

IllegalException, if fail to get node list.

### getTransactionInfoByBlockNum

Get transactionInfo from block number.  

**PARAMS**  

*1. blockNum(long)**  

blockNum is the block height.  

**RETURN**  

TransactionInfoList object.  

**THROWS**  

IllegalException, if the parameters are not correct or zero transaction included.

### getTransactionInfoById 

Query the transaction fee, block height by transaction id.

**PARAMS**  

*1. txID(String)**  

Transaction hash, i.e. transaction id.  

**RETURN**  

TransactionInfo object.  

**THROWS**  

IllegalException, if the parameters are not correct.

### getAccount 

Get account info by address.  

**PARAMS**  

*1. address(String)**  

address, default hexString.

**RETURN**  

Account object.  

**THROWS**  

IllegalException, if the parameters are not correct.

### listWitnesses 

List all witnesses that current API node is connected to.  

**RETURN**  

WitnessList object.  

## Solidity Node APIs

### getAccountSolidity  

Get solid account info by address.  

**PARAMS**  

*1. address(String)**  

address, default hexString.  

**RETURN**  

Account object.  

**THROWS**  

IllegalException, if the parameters are not correct.

### getNowBlockSolidity  

Query the latest solid block information. 

**RETURN**  

BlockExtention object.  

**THROWS**  

IllegalException, if fail to get now block.

### getTransactionByIdSolidity

Get transaction receipt info from a transaction id, must be in solid block.  

**PARAMS**  

*1. txID(String)**  

Transaction hash, i.e. transaction id.  

**RETURN**  

Transaction object.  

**THROWS**  

IllegalException, if the parameters are not correct.

### getRewardSolidity  

Get the rewards that the voter has not received.  

**PARAMS**  

*1. address(String)**  

address, default hexString.  

**RETURN**  

NumberMessage object.

