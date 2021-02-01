# Contract

A Contract object represents for a smart contract, includes mutator, accrssor and toString methods. 

The Contract class makes it easier to call function and make deployments.

A Builder class is inside for easily building a Contact object.

## Fields & Attributes

The fields and attributes are defined according to the protobuf in java-tron, refer to [Smart contract protobuf defination](https://developers.tron.network/docs/protobuf-definition).



## Contract Functions

With a contract address, you can build a Contract object, and parse the functions into human-readable texts. With the `toString` method, it's easy to read the functions and make calls.

```java
public void getSmartContractDemo() {
        TronClient client = TronClient.ofNile("3333333333333333333333333333333333333333333333333333333333333333");
        try {
           
            Contract cntr = client.getContract("THi2qJf6XmvTJSpZHc17HgQsmJop6kb3ia");
            for (ContractFunction cf : cntr.getFunctions()) {
                System.out.println(cf.toString());
            }
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }

```

and the result is:

```shell
# function allowance(address _owner, address _spender) view returns (uint256 remaining)
# function approve(address _spender, uint256 _amount) returns (bool success)
# function balanceOf(address _owner) view returns (uint256 balance)
# function decimals() view returns (uint8 )
# function name() view returns (string )
# function owner() view returns (address )
# function symbol() view returns (string )
# function totalSupply() view returns (uint256 theTotalSupply)
# function transfer(address _to, uint256 _amount) returns (bool success)
# function transferFrom(address _from, address _to, uint256 _amount) returns (bool success)
```

## Contract Deployment

It's easy to deploy your smart contract on the TRON network. A smart contract must be compiled successfully in any IDE to obtain the bytecode and ABI for deployment.

```java
 /**
     * deploy a smart contract with its bytecode and abi.
     * 
     * Except from the contract's abi and bytecode, other attributes
     * of a contract have default values. You may choose to set values to 
     * any other attributes base on needs. The optional attributes are commented out
     * in this demo, refer to {@link org.tron.tronj.client.contract.Contract}.
     * 
     * Notice that call_token_value is the amount of TRC-10 token to be deposited.
     * 
     * The demo contract is:
     * 
     * pragma solidity >=0.4.16 <0.8.0;
     * 
     * contract SimpleStorage {
     *     uint storedData;
     *     function set(uint x) public {
     *         storedData = x;
     *     }
     *     function get() public view returns (uint) {
     *         return storedData;
     *     }
     * }
     * 
     * @throws RuntimeException if deployment duplicating / owner and origin address don't match
     * @throws InvalidProtocolBufferException if the input is not valid JSON format or there are unknown fields in the input
     */
    public void deploySmartContract() {
        TronClient c = TronClient.ofNile("private key"); 
        try {
            String bytecode = "608060405234801561001057600080fd5b50d380156" +
                           "1001d57600080fd5b50d2801561002a57600080fd5" +
                           "b5060dd806100396000396000f3fe6080604052348" +
                           "015600f57600080fd5b50d38015601b57600080fd5" +
                           "b50d28015602757600080fd5b5060043610604a576" +
                           "0003560e01c806360fe47b114604f5780636d4ce63" +
                           "c14607a575b600080fd5b607860048036036020811" +
                           "015606357600080fd5b81019080803590602001909" +
                           "291905050506096565b005b608060a0565b6040518" +
                           "082815260200191505060405180910390f35b80600" +
                           "08190555050565b6000805490509056fea26474726" +
                           "f6e58206304a660fbed31eff4077ef91f51652f50c" +
                           "642cdd52d505857b0b3f17e51e1a864736f6c634300050e0031";

            String abi = "{\n" +
                "\t\"entrys\": [{\n" +
                "\t\t\"inputs\": [],\n" +
                "\t\t\"name\": \"get\",\n" +
                "\t\t\"outputs\": [{\n" +
                "\t\t\t\"internalType\": \"uint256\",\n" +
                "\t\t\t\"name\": \"\",\n" +
                "\t\t\t\"type\": \"uint256\"\n" +
                "\t\t}],\n" +
                "\t\t\"stateMutability\": \"view\",\n" +
                "\t\t\"type\": \"function\"\n" +
                "\t}, {\n" +
                "\t\t\"inputs\": [{\n" +
                "\t\t\t\"internalType\": \"uint256\",\n" +
                "\t\t\t\"name\": \"x\",\n" +
                "\t\t\t\"type\": \"uint256\"\n" +
                "\t\t}],\n" +
                "\t\t\"name\": \"set\",\n" +
                "\t\t\"outputs\": [],\n" +
                "\t\t\"stateMutability\": \"nonpayable\",\n" +
                "\t\t\"type\": \"function\"\n" +
                "\t}]\n" +
                "}";

            Contract cntr = new Contract.Builder()
                                    .setOwnerAddr(c.parseAddress("TFRgpvvNTe8bwC666D6orYhEkCcYsbax8U"))
                                    .setOriginAddr(c.parseAddress("TFRgpvvNTe8bwC666D6orYhEkCcYsbax8U"))
                                    .setBytecode(ByteString.copyFrom(Numeric.hexStringToByteArray(bytecode)))
                                    .setAbi(abi)
                                    // .setCallValue()
                                    // .setName()
                                    // .setConsumeUserResourcePercent()
                                    // .setOriginEnergyLimit()
                                    .build();
            cntr.setClient(c);
        
            TransactionBuilder builder = cntr.deploy();
            //use the following method with parameters to call if has any TRC-10 deposit
            //TransactionBuilder builder = cntr.deploy(tokenId, callTokenValue);
            builder.setFeeLimit(1000000000L);
            builder.setMemo("Let's go!");
            //sign transaction
            Transaction signedTxn = c.signTransaction(builder.build());
            System.out.println(signedTxn.toString());
            //broadcast transaction
            TransactionReturn ret = c.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

Note: personal private key is required for `setClient`.
