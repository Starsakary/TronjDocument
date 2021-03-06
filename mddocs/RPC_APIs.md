# RPC APIs  

This chapter describes the specific definitions, parameters, return values and exception handling of the RPC APIs.

## Full Node APIs  

**Note**  

To convert the raw result to a readable BASE58 address:

```java
Base58Check.bytesToBase58(client.parseAddress(client.toHex(byteAddress)).toByteArray())
```   

### signTransaction(TransactionExtention)

Sign a transactionExtention with the client binding private key.

**PARAMS**

*1. txnExt(TransactionExtention)*

> A TransactionExtention object.

**RETURN**

A signed transaction.

### signTransaction(Transaction)

Sign a transaction with the client binding private key.

**PARAMS**

*1. txn(Transaction)*

> A Transaction object.

**RETURN**

A signed transaction.

### signTransaction(TransactionExtention, Private key)

Sign a transaction with a private key.

**PARAMS**

*1. txn(Transaction)*

> A Transaction object.

*2. kp(SECP256K1.KeyPair)*

> The private key to sign the transaction.

**RETURN**

A signed transaction.

### signTransaction(Transaction, Private key)

Sign a transactionExtention with a private key.

**PARAMS**

*1. txnExt(TransactionExtention)*

> A TransactionExtention object.

*2. kp(SECP256K1.KeyPair)*

> The private key to sign the transaction.

**RETURN**

A signed transaction.

### generateAddress  

Generate a private key offline.

**RETURN**  

A private key.  

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

Transfer TRX. The amount is in SUN. 

**PARAMS**

*1. fromAddress(String)**  

> fromAddress is the owner address.

*2. toAddress(String)**  

> toAddress is the recipient address.  

*3. amount(int)**  

> amount is the amount of TRX to transfer in SUN.  

**RETURN**  

TransactionExtention, including execution results.  

**THROWS**  

IllegalException, if fail to transfer.  

**EXAMPLE** 

```java
public void sendTrx() {
        System.out.println("============= TRC transfer =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            TransactionExtention transaction = client.transfer("owner address", "TP8LKAf3R3FHDAcrQXuwBEWmaGrrUdRvzb", 1_000_000);
            Transaction signedTxn = client.signTransaction(transaction);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### TransferTrc10

Transfers TRC10 Asset  

**PARAMS**  

*1. fromAddress(String)**  

> fromAddress is the owner address.  

*2. toAddress(String)**  

> toAddress is the recipient address.  

*3. tokenId(int)**  

> asset name

*4. amount(int)**  

> amount is the amount of TRX to transfer in SUN.  

**RETURN**  

TransactionExtention, including execution results.  

**THROWS**  

IllegalException, if fail to transfer trc10.  

**EXAMPLE** 
 
```java
public void transferTrc10(){
        System.out.println("============= transferTrc10 =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            TransactionExtention transactionExtention = client.transferTrc10("owner address", "TP8LKAf3R3FHDAcrQXuwBEWmaGrrUdRvzb",
                    1000016, 1_000_000);
            Transaction signedTxn = client.signTransaction(transactionExtention);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### freezeBalance

Freeze balance to get votes and energy or bandwidth, default for 3 days.  

**PARAMS**  

*1. ownerAddress(String)**  

> owner address, default hexString.  

*2. frozenBalance(long)**  

> TRX freeze amount, the unit is sun.

*3. frozenDuration(long)**  

> TRX freeze duration, only be specified as 3 days.

*4. resourceCode(int)**  

> resource type, can be "ENERGY" or "BANDWIDTH"  

*5. receiveAddress(String)*  

> optional, the address that will receive the resource, default hexString 

**RETURN**  

TransactionExtention, including execution results.  

**THROWS**  

IllegalException, if fail to freeze balance.  

**EXAMPLE** 
 
```java
public void freezeBalance() {
        System.out.println("============= freeze balance =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            TransactionExtention transaction = client.freezeBalance("owner address", 1_000_000L, 3L,1);
            Transaction signedTxn = client.signTransaction(transaction);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### unfreezeBalance 

Unfreeze the frozen TRX.  

**PARAMS**  

*1. ownerAddress(String)**  

> owner address, default hexString.  

*2. resourceCode(int)**  

> resource type, can be "ENERGY" or "BANDWIDTH"  

*3. receiveAddress(String)*  

> optional, the address that will receive the resource, default hexString 

**RETURN**  

TransactionExtention, including execution results.  

**THROWS**  

IllegalException, if fail to unfreeze balance.  

**EXAMPLE** 
 
```java
public void unFreezeBalance() {
        System.out.println("============= unFreeze balance =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            TransactionExtention transaction = client.unfreezeBalance("owner address", 1);
            Transaction signedTxn = client.signTransaction(transaction);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### voteWitness 

Vote for a witness

**PARAMS**  

*1. ownerAddress(String)**  

> owner address, default hexString.  

*2. votes(Map)**  

> key: 'vote_address' stands for the address of the witness you want to vote, default hexString.  

> value: 'vote_count' stands for the number of votes you want to vote.

**RETURN**  

TransactionExtention, including execution results.  

**THROWS**  

IllegalException, if fail to vote witness.  

**EXAMPLE** 
 
```java
public void voteWitness(){
        System.out.println("============= voteWitness =============");
        HashMap<String, String> witness = new HashMap<>();
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            witness.put("TG7RHXaL7E9rqSkBavX7s1vtikoz6np6bD","1");
            TransactionExtention transaction = client.voteWitness("owner address",witness);
            Transaction signedTxn = client.signTransaction(transaction);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### createAccount 

Create an account. Uses an already activated account to create a new account

**PARAMS**  

*1. ownerAddress(String)**  

> owner address, an activated account, default hexString.  

*2. accountAddress(String)**  

> account address, the address of the new account, default hexString.  

**RETURN**  

TransactionExtention, including execution results.  

**THROWS**  

IllegalException, if fail to create account.  

**EXAMPLE** 
 
```java
public void createAccount(){
        System.out.println("============= createAccount =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            TransactionExtention transaction = client.createAccount("owner address","account address");
            System.out.println(transaction);
            Transaction signedTxn = client.signTransaction(transaction);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### updateAccount 

Modify account name, you can update it only if the name is empty

**PARAMS**  

*1. address(String)**  

> owner address, default hexString.  

*2. accountName(String)**  

> the name of the account.  

**RETURN**  

TransactionExtention, including execution results.  

**THROWS**  

IllegalException, if fail to update account name.  

**EXAMPLE** 
 
```java
public void updateAccount(){
        System.out.println("============= updateAccount =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            TransactionExtention transaction = client.updateAccount("owner address","updatename");
            System.out.println(transaction);
            Transaction signedTxn = client.signTransaction(transaction);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
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

IllegalException, if fail to get the latest block.  

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

**PARAMS**  

*1. blockNum(long)**  

> blockNum is the block height.  

**RETURN**  

Block object.  

**THROWS**  

IllegalException, if the parameters are not correct(e.g. block does not exist).  

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

**PARAMS**  

*1. num(long)**  

> Number of latest blocks. It must be between 1 and 99.    

**RETURN**  

BlockListExtention object.  

**THROWS**  

IllegalException, if the parameter is not between 1 and 99.  

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

### getBlockByLimitNext 

Returns the list of Block Objects included in the 'Block Height' range specified.  

**PARAMS**  

*1. startNum(long)**  

> Number of start block height, including this block.    

*2. endNum(long)**  

> Number of end block height, excluding this block

**RETURN**  

BlockListExtention object.  

**THROWS**  

IllegalException, if the parameters are not correct or the difference between startNum and endNum is greater than 100.  

**EXAMPLE** 
 
```java
public void getBlockByLimitNext() {
        System.out.println("============= getBlockByLimitNext =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            BlockListExtention blockListExtention = client.getBlockByLimitNext(0,10);
            System.out.println(blockListExtention);
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getChainParameters

All parameters that the blockchain committee can set.  

**RETURN**  

ChainParameters object.  

**THROWS**  

IllegalException, if fail to get chain parameters.  

**EXAMPLE** 
 
```java
public void getChainParameters() {
        System.out.println("============= getChainParameters =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getChainParameters());
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

**PARAMS**  

*1. blockNum(long)**  

> blockNum is the block height.  

**RETURN**  

TransactionInfoList object.  

**THROWS**  

IllegalException, if the parameters are not correct or zero transaction included.

**EXAMPLE** 
 
```java
public void getTransactionInfoByBlockNum() {
        System.out.println("============= getTransactionInfoByBlockNum =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getTransactionInfoByBlockNum(11359274));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```


### getTransactionInfoById 

Query the transaction fee, block height by transaction id.

**PARAMS**  

*1. txID(String)**  

> Transaction hash, i.e. transaction id.  

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
            System.out.println(client.getTransactionInfoById("786c7516df88941e33ea44f03e637bd8c1ddcfd058634574102c6e3cfb93de0d"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getTransactionById 

Query transaction information by transaction id.

**PARAMS**  

*1. txID(String)**  

> Transaction hash, i.e. transaction id.  

**RETURN**  

Transaction object.  

**THROWS**  

IllegalException, if the parameters are not correct.  

**EXAMPLE** 
 
```java
public void getTransactionById() {
        System.out.println("============= getTransactionById =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getTransactionById("786c7516df88941e33ea44f03e637bd8c1ddcfd058634574102c6e3cfb93de0d"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getAccount 

Get account info by address.  

**PARAMS**  

*1. address(String)**  

> address, default hexString.

**RETURN**  

Account object.    

**EXAMPLE** 
 
```java
public void getAccount(){
        System.out.println("============= getAccount =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getAccount("TKwVM5tsELuTE3a5SUCWiQyVtEgxejL5Wj"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getAccountResource 

Query the resource information of an account(bandwidth,energy,etc).  

**PARAMS**  

*1. address(String)**  

> address, default hexString.

**RETURN**  

AccountResourceMessage object.    

**EXAMPLE** 
 
```java
public void getAccountResource(){
        System.out.println("============= getAccountResource =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getAccountResource("TKwVM5tsELuTE3a5SUCWiQyVtEgxejL5Wj"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getAccountNet 

Query bandwidth information.  

**PARAMS**  

*1. address(String)**  

> address, default hexString.

**RETURN**  

AccountNetMessage object.    

**EXAMPLE** 
 
```java
public void getAccountNet(){
        System.out.println("============= getAccountNet =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getAccountNet("TKwVM5tsELuTE3a5SUCWiQyVtEgxejL5Wj"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getAssetIssueList 

Query the list of all the TRC10 tokens.  

**RETURN**  

AssetIssueList object.    

**EXAMPLE** 
 
```java
public void getAssetIssueList() {
        System.out.println("============= getAssetIssueList =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getAssetIssueList());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getAssetIssueByAccount 

Query the TRC10 token information issued by an account.  

**PARAMS**  

*1. address(String)**  

> address, the Token Issuer account address, default hexString.

**RETURN**  

AssetIssueList object.    

**EXAMPLE** 
 
```java
public void getAssetIssueByAccount() {
        System.out.println("============= getAssetIssueByAccount =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getAssetIssueByAccount("TMX73eFtUtyZXg62uCnjSywDu7pD5sau48"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

### getAssetIssueById 

Query a token by token id.  

**PARAMS**  

*1. assetId(String)**  

> the ID of the TRC10 token.

**RETURN**  

AssetIssueContract object.    

**EXAMPLE** 
 
```java
public void getAssetIssueById() {
        System.out.println("============= getAssetIssueById =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getAssetIssueById("1000134"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  

### getExchangeById 

Query exchange pair based on id.  

**PARAMS**  

*1. id(String)**  

> transaction pair id.

**RETURN**  

Exchange object.    

**THROWS**  

IllegalException, if fail to get exchange pair. 

**EXAMPLE** 
 
```java
public void getExchangeById() {
        System.out.println("============= getExchangeById =============");
        TronClient client = TronClient.ofMainnet("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getExchangeById("10"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
``` 

### listExchanges 

List all exchange pairs.  

**RETURN**  

ExchangeList object.  

**EXAMPLE** 
 
```java
public void listExchanges(){
        System.out.println("============= listExchanges =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.listExchanges());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
``` 

### getProposalById 

Query proposal based on ID.  

**PARAMS**  

*1. id(String)**  

> proposal id.

**RETURN**  

Proposal object.    

**THROWS**  

IllegalException, if fail to get proposal. 

**EXAMPLE** 
 
```java
public void getProposalById() {
        System.out.println("============= getProposalById =============");
        TronClient client = TronClient.ofMainnet("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getProposalById("15"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
``` 

### getPaginatedAssetIssueList 

Query the list of all the tokens by pagination.

**PARAMS**  

*1. offset(long)**  

> the index of the start token.

*2. limit(long)**  

> the amount of tokens per page.

**RETURN**  

AssetIssueList object.     

**EXAMPLE** 
 
```java
public void getPaginatedAssetIssueList() {
        System.out.println("============= getPaginatedAssetIssueList =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getPaginatedAssetIssueList(0,20));
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

### getDelegatedResourceAccountIndex 

Query the energy delegation by an account. i.e. list all addresses that have delegated resources to an account 

**PARAMS**  

*1. address(String)**  

> address, default hexString.

**RETURN**  

DelegatedResourceAccountIndex object.     

**EXAMPLE** 
 
```java
public void getDelegatedResourceAccountIndex(){
        System.out.println("============= getDelegatedResourceAccountIndex =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            DelegatedResourceAccountIndex accountIndex = client.getDelegatedResourceAccountIndex("TLtrDb1udekjDumnrf3EVeke3Q6pHkZxjm");
            ByteString account = accountIndex.getAccount();

            System.out.println("Accounts: "+Base58Check.bytesToBase58(client.parseAddress(client.toHex(account)).toByteArray()));

            int fromAccountsCount = accountIndex.getFromAccountsCount();
            for(int i =0;i<fromAccountsCount;i++){
                ByteString fromAccounts = accountIndex.getFromAccounts(i);
                System.out.println("fromAccounts: "+Base58Check.bytesToBase58(client.parseAddress(client.toHex(fromAccounts)).toByteArray()));
            }

            int toAccountsCount = accountIndex.getToAccountsCount();
            for(int i =0;i<toAccountsCount;i++){
                ByteString toAccounts = accountIndex.getToAccounts(i);
                System.out.println("toAccounts: "+Base58Check.bytesToBase58(client.parseAddress(client.toHex(toAccounts)).toByteArray()));
            }

        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```   

### getDelegatedResource 

Returns all resources delegations from an account to another account. The fromAddress can be retrieved from the GetDelegatedResourceAccountIndex API 

**PARAMS**  

*1. fromAddress(String)**  

> energy from address, default hexString.  

*2. toAddress(String)**  

> energy delegation information, default hexString.

**RETURN**  

DelegatedResourceList object.     

**EXAMPLE** 
 
```java
public void getDelegatedResource(){
        System.out.println("============= getDelegatedResource =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getDelegatedResource("TLtrDb1udekjDumnrf3EVeke3Q6pHkZxjm","TMmbeRPnFhXC7BPLaF2M1HCsoE4jwZNB7b"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  

### createAssetIssue 

Issue a token 

**PARAMS**  

*1. ownerAddress(String)**  

> Owner address, default hexString.  

*2. name(String)**  

> Token name, default hexString.  

*3. abbr(String)**  

> Token name abbreviation, default hexString.  

*4. totalSupply(long)**  

> Token total supply.  

*5. trxNum(int)**  

> Define the price by the ratio of trx_num/num.  

*6. icoNum(int)**  

> Define the price by the ratio of trx_num/num.  

*7. startTime(long)**  

> ICO start time.  

*8. endTime(long)**  

> ICO end time.  

*9. url(String)**  

> Token official website url, default hexString.  

*10. freeAssetNetLimit(long)**  

> Token free asset net limit.  

*11. publicFreeAssetNetLimit(long)**  

> Token public free asset net limit.  

*12. precision(int)**    

*13. frozenSupply*  

> HashMap<frozenDay, frozenAmount>.   

*14. description(String)**  

> Token description, default hexString.     

**RETURN**  

TransactionExtention object.  

**THROWS**  

IllegalException, if fail to create AssetIssue.     

**EXAMPLE** 
 
```java
public void createAssetIssue() {
        System.out.println("============= createAssetIssue =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            long start = System.currentTimeMillis() + 2000;
            long end = System.currentTimeMillis() + 1000000000;
            HashMap<String, String> frozenSupply = new HashMap<String, String>();
            frozenSupply.put("1","1");
            frozenSupply.put("2","1");
            frozenSupply.put("3","2");
            TransactionExtention transaction = client.createAssetIssue("owner address","lsp1", "saf1",100000000L, 1, 1000,start,end,"7777772e6578616d706c652e636f6d",
                                100000L,1L,6,frozenSupply,"stest-assetissue");
            System.out.println(transaction);
            Transaction signedTxn = client.signTransaction(transaction);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  

### participateAssetIssue 

Participate a token 

**PARAMS**  

*1. toAddress(String)**  

> the issuer address of the token, default hexString.  

*2. ownerAddress(String)**  

> the participant address, default hexString.  

*3. assertName(String)**  

> token id, default hexString.  

*4. amount(long)**  

> participate token amount.

**RETURN**  

TransactionExtention object.  

**THROWS**  

IllegalException, if fail to participate AssetIssue.      

**EXAMPLE** 
 
```java
public void participateAssetIssue() {
        System.out.println("============= participateAssetIssue =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {

            TransactionExtention transaction = client.participateAssetIssue("toAddress","ownerAddress","1000204", 1L);
            System.out.println(transaction);
            Transaction signedTxn = client.signTransaction(transaction);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  

### updateAsset 

Update basic TRC10 token information 

**PARAMS**  

*1. ownerAddress(String)**  

> owner address, default hexString.  

*2. description(String)**  

> the description of token, default hexString.  

*3. url(String)**  

> The token's website url, default hexString.  

*4. newLimit(int)**  

> each token holder's free bandwidth.  

*5. newPublicLimit(int)**  

> the total free bandwidth of the token.  

**RETURN**  

TransactionExtention object.  

**THROWS**  

IllegalException, if fail to update asset.      

**EXAMPLE** 
 
```java
public void updateAsset() {
        System.out.println("============= updateAsset =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {

            TransactionExtention transaction = client.updateAsset("ownerAddress","newlsp","sadf", 1,2);
            System.out.println(transaction);
            Transaction signedTxn = client.signTransaction(transaction);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```   

### unfreezeAsset 

Unfreeze a token that has passed the minimum freeze duration 

**PARAMS**  

*1. ownerAddress(String)**  

> owner address, default hexString.      

**RETURN**  

TransactionExtention object.  

**THROWS**  

IllegalException, if fail to unfreeze asset.      

**EXAMPLE** 
 
```java
public void unfreezeAsset() {
        System.out.println("============= unfreezeAsset =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {

            TransactionExtention transaction = client.unfreezeAsset("ownerAddress");
            System.out.println(transaction);
            Transaction signedTxn = client.signTransaction(transaction);
            System.out.println(signedTxn.toString());
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  
   
### getAssetIssueByAccount 

Query the TRC10 token information issued by an account 

**PARAMS**  

*1. address(String)**  

> the Token Issuer account address.      

**RETURN**  

AssetIssueList object.        

**EXAMPLE** 
 
```java
public void getAssetIssueByAccount() {
        System.out.println("============= getAssetIssueByAccount =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            System.out.println(client.getAssetIssueByAccount("address"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  

### getAssetIssueByName 

Query a token by token name 

**PARAMS**  

*1. name(String)**  

> the name of the TRC10 token.      

**RETURN**  

AssetIssueContract object.        

**EXAMPLE** 
 
```java
public void getAssetIssueByName() {
        System.out.println("============= getAssetIssueByName =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            System.out.println(client.getAssetIssueByName("name"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  

### getAssetIssueListByName 

Query the list of all the TRC10 tokens by token name 

**PARAMS**  

*1. name(String)**  

> the name of the TRC10 token.      

**RETURN**  

AssetIssueList object.        

**EXAMPLE** 
 
```java
public void getAssetIssueListByName() {
        System.out.println("============= getAssetIssueListByName =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {
            System.out.println(client.getAssetIssueByName("name"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  
 
### getTransactionSignWeight 

Query transaction sign weight 

**PARAMS**  

*1. trx(Transaction)**  

> transaction object.      

**RETURN**  

TransactionSignWeight object.        

**EXAMPLE** 
 
```java
public void getTransactionSignWeight() {
        System.out.println("============= getTransactionSignWeight =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {

            TransactionSignWeight transaction = client.getTransactionSignWeight(client.getTransactionById("786c7516df88941e33ea44f03e637bd8c1ddcfd058634574102c6e3cfb93de0d"));
            System.out.println(transaction);
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  

### getTransactionApprovedList 

Query transaction approvedList 

**PARAMS**  

*1. trx(Transaction)**  

> transaction object.      

**RETURN**  

TransactionApprovedList object.        

**EXAMPLE** 
 
```java
public void getTransactionApprovedList() {
        System.out.println("============= getTransactionApprovedList =============");
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {

            TransactionApprovedList transaction = client.getTransactionApprovedList(client.getTransactionById("786c7516df88941e33ea44f03e637bd8c1ddcfd058634574102c6e3cfb93de0d"));
            System.out.println(transaction);
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```  

### accountPermissionUpdate 

Update the account permission, includes the three privilege levels of owner, witness, and active privilege. 

**PARAMS**  

*1. contract(AccountPermissionUpdateContract)**  

> AccountPermissionUpdateContract object.      

**RETURN**  

TransactionExtention object.  

**THROWS**  

IllegalException, if fail to update account permission.      

**EXAMPLE** 
 
```java
public void accountPermissionUpdate(){
        System.out.println("============= accountPermissionUpdate =============");
        TronClient client = TronClient.ofNile("your privateKey");
        try {

        AccountPermissionUpdateContract.Builder builder =
                AccountPermissionUpdateContract.newBuilder();

        Permission ownerPermission = null;
        Permission.Builder builderOwner =
                Permission.newBuilder();
        builderOwner.setTypeValue(0);
        builderOwner.setPermissionName("owner");
        builderOwner.setThreshold(1);
        Common.Key.Builder keyOwner =
                Common.Key.newBuilder();
        keyOwner.setAddress(client.parseAddress("4122ED62967ED0D2186B2FC639D7972D172A95C855"));
        keyOwner.setWeight(1);
        builderOwner.addKeys(keyOwner);
        ownerPermission = builderOwner.build();

        Permission activePermissions = null;
        Permission.Builder builderActive =
                Permission.newBuilder();
        builderActive.setTypeValue(2);
        builderActive.setThreshold(1);
        builderActive.setPermissionName("active0");
        builderActive.setOperations(client.parseAddress("7fff1fc0037e0000000000000000000000000000000000000000000000000000"));
        Common.Key.Builder keyActive =
                Common.Key.newBuilder();
        keyActive.setAddress(client.parseAddress("4122ED62967ED0D2186B2FC639D7972D172A95C855"));
        keyActive.setWeight(1);
        builderActive.addKeys(keyActive);
        activePermissions = builderActive.build();

        builder.setOwner(ownerPermission);
        builder.addActives(activePermissions);
        builder.setOwnerAddress(client.parseAddress("ownerAddress"));

        TransactionExtention transaction = client.accountPermissionUpdate(builder.build());
        System.out.println(transaction);
        Transaction signedTxn = client.signTransaction(transaction);
        System.out.println(signedTxn.toString());
        TransactionReturn ret = client.broadcastTransaction(signedTxn);
        System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

## Solidity Node APIs

### getAccountSolidity  

Get solid account info by address.  

**PARAMS**  

*1. address(String)**  

> address, default hexString.  

**RETURN**  

Account object.   

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

**PARAMS**  

*1. txID(String)** 

> Transaction hash, i.e. transaction id.  

**RETURN**  

Transaction object.  

**THROWS**  

IllegalException, if the parameters are not correct(e.g. the specified transaction has not been solidified).  

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

**PARAMS**  

*1. address(String)**  

> address, default hexString.  

**RETURN**  

NumberMessage object.  

**EXAMPLE** 
 
```java
public void getRewardSolidity(){
        System.out.println("============= getRewardSolidity =============");
        TronClient client = TronClient.ofShasta("3333333333333333333333333333333333333333333333333333333333333333");
        try {
            System.out.println(client.getRewardSolidity("TKryTFSUB2UY8jMVc3Rz3ofiUPrnR6pRAs"));
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```