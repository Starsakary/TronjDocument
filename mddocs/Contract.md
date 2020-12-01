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

```java
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

