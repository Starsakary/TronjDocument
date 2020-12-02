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

**EXAMPLE** 
 
```java
public void generateAddress() {
        System.out.println("============= generateAddress =============");
        try {
            System.out.println(TronClient.generateAddress());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

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

**EXAMPLE** 

```java
public void sendTrx() {
        System.out.println("============= TRC transfer =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.transfer("TJvznvPr4iGRT2FL7ZBHKLGWt7QgScWLnw", "TP8LKAf3R3FHDAcrQXuwBEWmaGrrUdRvzb", 1_000_000));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### TransferTrc10

Transfers TRC10 Asset  

**BODY PARAMS**  

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

**EXAMPLE** 
 
```java
public void transferTrc10(){
        System.out.println("============= transferTrc10 =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.transferTrc10("TDCm25Qp6y6r3Q52qr8m3YQk8VMX1MhBu9", "TP8LKAf3R3FHDAcrQXuwBEWmaGrrUdRvzb",
                    1000322, 1_000_000));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### freezeBalance

Freeze balance to get energy or bandwidth, for 3 days.  

**BODY PARAMS**  

*1. ownerAddress(String)**  

owner address, default hexString.  

*2. frozenBalance(long)**  

TRX freeze amount, the unit is sun.

*3. frozenDuration(long)**  

TRX freeze duration, only be specified as 3 days.

*4. resourceCode(int)**  

resource type, can be "ENERGY" or "BANDWIDTH"

**RETURN**  

Transaction, including execution results.  

**THROWS**  

IllegalException, if fail to freeze balance.  

**EXAMPLE** 
 
```java
public void freezeBalance() {
        System.out.println("============= freeze balance =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.freezeBalance("TT7wFAHH3LJnhAZMPVEQXm6MCjTJvkh4HP", 1_000_000L, 3L,1));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### unfreezeBalance 

Unfreeze balance to get TRX back.  

**BODY PARAMS**  

*1. ownerAddress(String)**  

owner address, default hexString.  

*2. resourceCode(int)**  

resource type, can be "ENERGY" or "BANDWIDTH"  

**RETURN**  

Transaction, including execution results.  

**THROWS**  

IllegalException, if fail to unfreeze balance.  

**EXAMPLE** 
 
```java
public void unFreezeBalance() {
        System.out.println("============= unFreeze balance =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");

        try {
            System.out.println(client.unfreezeBalance("TJE6PX8c66Tvn8sR9VtybcmB7EYSjUYMzZ", 1));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### voteWitness 

Vote for witnesses  

**BODY PARAMS**  

*1. ownerAddress(String)**  

owner address, default hexString.  

*2. votes(Map)**  

key: 'vote_address' stands for the address of the witness you want to vote, default hexString.  

value: 'vote_count' stands for the number of votes you want to vote.

**RETURN**  

Transaction, including execution results.  

**THROWS**  

IllegalException, if fail to vote witness.  

**EXAMPLE** 
 
```java
public void voteWitness(){
        System.out.println("============= voteWitness =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        HashMap<String, String> witness = new HashMap<>();
        witness.put("41F16412B9A17EE9408646E2A21E16478F72ED1E95","-1");
        try {
            System.out.println(client.voteWitness("TJRabPrwbZy45sbavfcjinPJC18kjpRTv8",witness));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getNowBlock

Query the latest block information. 

**RETURN**  

Block object.  

**THROWS**  

IllegalException, if fail to get now block.  

**EXAMPLE** 
 
```java
public void getNowBlock() {
        System.out.println("============= getNowBlock =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getNowBlock());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getBlockByNum  

Returns the Block Object corresponding to the 'Block Height' specified (number of blocks preceding it).  

**BODY PARAMS**  

*1. blockNum(long)**  

blockNum is the block height.  

**RETURN**  

Block object.  

**THROWS**  

IllegalException, if the parameters are not correct.  

**EXAMPLE** 
 
```java
public void getBlockByNum() {
        System.out.println("============= getBlockByNum =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            Block block = client.getBlockByNum(10);
            System.out.println(block);
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getBlockByLatestNum 

Get some latest blocks.  

**BODY PARAMS**  

*1. num(long)**  

Number of latest blocks.  

**RETURN**  

BlockListExtention object.  

**THROWS**  

IllegalException, if the parameters are not correct.  

**EXAMPLE** 
 
```java
public void getBlockByLatestNum() {
        System.out.println("============= getBlockByLatestNum =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            BlockListExtention blockListExtention = client.getBlockByLatestNum(10);
            System.out.println(blockListExtention);
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getNodeInfo

Get current API node’ info.  

**RETURN**  

NodeInfo object.  

**THROWS**  

IllegalException, if fail to get nodeInfo.  

**EXAMPLE** 
 
```java
public void getNodeInfo() {
        System.out.println("============= getNodeInfo=============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getNodeInfo());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### listNodes

List all nodes that current API node is connected to.  

**RETURN**  

NodeList object.  

**THROWS**  

IllegalException, if fail to get node list.  

**EXAMPLE** 
 
```java
public void listNodes() {
        System.out.println("============= listNodes=============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.listNodes());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getTransactionInfoByBlockNum

Get transactionInfo from block number.  

**BODY PARAMS**  

*1. blockNum(long)**  

blockNum is the block height.  

**RETURN**  

TransactionInfoList object.  

**THROWS**  

IllegalException, if the parameters are not correct.  

**EXAMPLE** 
 
```java
public void getTransactionInfoByBlockNum() {
        System.out.println("============= getTransactionInfoByBlockNum =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getTransactionInfoByBlockNum(1));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getTransactionInfoById 

Query the transaction fee, block height by transaction id.

**BODY PARAMS**  

*1. txID(String)**  

Transaction hash, i.e. transaction id.  

**RETURN**  

TransactionInfo object.  

**THROWS**  

IllegalException, if the parameters are not correct.  

**EXAMPLE** 
 
```java
public void getTransactionInfoById() {
        System.out.println("============= getTransactionInfoById =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getTransactionInfoById("bd9f37ace1688853ae0db305aa385761e8ae5517a663ec7ef3ed08ea09b393b7"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getAccount 

Get account info by address.  

**BODY PARAMS**  

*1. address(String)**  

address, default hexString.

**RETURN**  

Account object.  

**THROWS**  

IllegalException, if the parameters are not correct.  

**EXAMPLE** 
 
```java
public void getAccount(){
        System.out.println("============= getAccount =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getAccount("TJvznvPr4iGRT2FL7ZBHKLGWt7QgScWLnw"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### listWitnesses 

List all witnesses that current API node is connected to.  

**RETURN**  

WitnessList object.  

**EXAMPLE** 
 
```java
public void listWitnesses(){
        System.out.println("============= listWitnesses =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.listWitnesses());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  

## Solidity Node APIs

### getAccountSolidity  

Get solid account info by address.  

**BODY PARAMS**  

*1. address(String)**  

address, default hexString.  

**RETURN**  

Account object.  

**THROWS**  

IllegalException, if the parameters are not correct.  

**EXAMPLE** 
 
```java
public void getAccountSolidity(){
        System.out.println("============= getAccountSolidity =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getAccountSolidity("TKwVM5tsELuTE3a5SUCWiQyVtEgxejL5Wj"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  

### getNowBlockSolidity  

Query the latest solid block information. 

**RETURN**  

BlockExtention object.  

**THROWS**  

IllegalException, if fail to get now block.  

**EXAMPLE** 
 
```java
public void getNowBlockSolidity() {
        System.out.println("============= getNowBlockSolidity =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getNowBlockSolidity());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getTransactionByIdSolidity

Get transaction receipt info from a transaction id, must be in solid block.  

**BODY PARAMS**  

*1. txID(String)** 

Transaction hash, i.e. transaction id.  

**RETURN**  

Transaction object.  

**THROWS**  

IllegalException, if the parameters are not correct.  

**EXAMPLE** 
 
```java
public void getTransactionByIdSolidity() {
        System.out.println("============= getTransactionByIdSolidity =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getTransactionByIdSolidity("3535304212e0090d421ec88cd194d35875b748c0ad453fcde6d7b4d43e852ced"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getRewardSolidity  

Get the rewards that the voter has not received.  

**BODY PARAMS**  

*1. address(String)**  

address, default hexString.  

**RETURN**  

NumberMessage object.  

**EXAMPLE** 
 
```java
public void getRewardSolidity(){
        System.out.println("============= getRewardSolidity =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getRewardSolidity("TNSdGcMvSDwksounpkN1ZCmA7V5QLEDtWu"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```