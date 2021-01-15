# Smart Contract

Smart contract is a key feature of TRON network. It's easy to creating and interacting with smart contracts through Tronj.

## Calling smart contract

There are two types of smart contract calls: *const call* and *trigger call*.

Simply, a *const call* returns result immediately once and no need to sign or broadcast.

*Trigger call* is a type of system contract call, needs signing and broadcasting. It fetches the result through the API.

You may see the functions of a specified contract by `ContractFunction.toString()`, then you can construct a `Function` object to make a constant/trigger call.

### Get contract

```java
public void getSmartContract() {
        TronClient client = TronClient.ofNile("Your private key");
        try {
          
            Contract cntr = client.getContract("TF17BgPaZYbz8oxbjhriubPDsA7ArKoLX3"); //JST
            System.out.println("Contract name: " + cntr.getName());
            System.out.println("Contract functions: " + cntr.getFunctions().size());
            for (ContractFunction cf : cntr.getFunctions()) {
                System.out.println(cf.toString());
            }
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

The result is:

```shell
Contract name: JST
Contract functions: 26
# function name() view returns (string )
# function stop() returns (null )
# function approve(guy address, wad uint256 ) returns (bool )
# function setOwner(owner_ address ) returns (null )
# function totalSupply() view returns (uint256 )
# function transferFrom(src address, dst address, wad uint256 ) returns (bool )
# function decimals() view returns (uint256 )
# function mint(guy address, wad uint256 ) returns (null )
# function burn(wad uint256 ) returns (null )
# function balanceOf(src address ) view returns (uint256 )
# function stopped() view returns (bool )
# function setAuthority(authority_ address ) returns (bool result)
# function owner() view returns (address )
# function symbol() view returns (string )
# function burn(guy address, wad uint256 ) returns (null )
# function mint(wad uint256 ) returns (null )
# function transfer(dst address, wad uint256 ) returns (bool )
# function push(dst address, wad uint256 ) returns (null )
# function setSymbol(symbol_ string ) returns (null )
# function move(src address, dst address, wad uint256 ) returns (null )
# function start() returns (null )
# function authority() view returns (address )
# function setName(name_ string ) returns (null )
# function approve(guy address ) returns (bool )
# function allowance(src address, guy address ) view returns (uint256 )
# function pull(src address, wad uint256 ) returns (null )
```

### Trigger call

The first half of the *trigger call* process is similar to the *const call*.

You can easily set `feeLimit`, `memo` and other common attributes via the `TransactionBuilder`.

```java
public void triggerCallDemo() {
        TronClient client = TronClient.ofNile("your private key");
        try {
            //function 'transfer'
            //params: function name, function params
            Function trc20Transfer = new Function("transfer",
            Arrays.asList(new Address("to address"),
                new Uint256(BigInteger.valueOf(10L).multiply(BigInteger.valueOf(10).pow(18)))),
            Arrays.asList(new TypeReference<Bool>() {}));

            //the params are: owner address, contract address, function
            TransactionBuilder builder = client.triggerCall("owner address", "contract address", trc20Transfer); //JST
            //set extra params
            builder.setFeeLimit(100000000L);
            builder.setMemo("Let's go!");
            //sign transaction
            Transaction signedTxn = client.signTransaction(builder.build());
            System.out.println(signedTxn.toString());
            //broadcast transaction
            TransactionReturn ret = client.broadcastTransaction(signedTxn);
            System.out.println("======== Result ========\n" + ret.toString());
        } catch (Exception e) {
            System.out.println("error: " + e);
        }
    }
```

## Smart contract APIs

### getContract

Get a [Contract](Contract.md) object from the address.

**PARAMS**

*1. contractAddress(String)*

The address of a smart contract.

**RETURN**

A Contract object.

**THROW**

Throws if the given contract address does not match any.

### constantCall

make a constant call, without broadcasting.

**PARAMS**  

*1. ownerAddr(String)**  

The caller's address.

*2. contractAddr(String)**  

The contract's address.  

*3. function(Function)**  

The exact function you are calling, you can find the example in the `triggerCallDemo`.

**RETURN**  

A TransactionExtention object

**THROW**

Throws if the function does not match any in the smart contract.

### triggerCall

Make a trigger call. Trigger calls require signature and broadcasting. Refer to [RPC_APIs](RPC_APIs.md) for the signing and broadcasting functions.

**PARAMS**

*1. ownerAddr(String)**  

The caller's address.

*2. contractAddr(String)**  

The contract's address.  

*3. function(Function)**  

The exact function you are calling, you can find the example in the `triggerCallDemo`.

**RETURN**  

A TransactionBuilder object, for easily setting memos, feelimit, Etc.

**THROW**

Throws if the function does not match any in the smart contract.

## TRC-20 

[TRC-20](https://github.com/tronprotocol/TIPs/blob/master/tip-20.m%64) is a standard interface for tokens, which has a set of standard functions.

Tronj implements the standard function calls defined in TIP-20.

Before you call the functions, a `TronClient` object should be initialized.

```java
TronClient c = TronClient.ofNile("Private key");
```

**NOTE: The `Trc20Contract` can be only created by a TRC-20 contract, or there will be errors while calling functions.**

### function name()

Returns the name of the token - e.g. `"MyToken"`.

**RETURN**

The token name as a `String`.

**EXAMPLE**

```java
public void getName() {
        Contract cntr = c.getContract("Contract address");
  //init a Trc20Contract object
  //params: contract, caller's address, TronClient
        Trc20Contract token = new Trc20Contract(cntr, "Caller address", c);
        try {
            System.out.println("Name: " + token.name());
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
```



### function symbol()

Returns the symbol of the token. E.g. `"HIX"`.

**RETURN**

The token symbol as a `String`.

**EXAMPLE**

```java
public void getSymbol() {
        Contract cntr = c.getContract("Contract address");
        Trc20Contract token = new Trc20Contract(cntr, "Caller address", c);
        try {
            System.out.println("Symbol: " + token.symbol());
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
```





### function decimals()

Returns the number of decimals the token uses - e.g. `8`, means to divide the token amount by `100000000` to get its user representation.

**RETURN**

The token's decimals as a `Biginteger`.

**EXAMPLE**

```java
public void getDecimals() {
        Contract cntr = c.getContract("Contract address");
        Trc20Contract token = new Trc20Contract(cntr, "Caller address", c);
        try {
            System.out.println("Decimals: " + token.decimals());
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
```



### function totalSupply()

Returns the total token supply.

**RETURN**

The total supply of the token as a `Biginteger`.

**EXAMPLE**

```java
public void getTotalSupply() {
        Contract cntr = c.getContract("Contract address");
        Trc20Contract token = new Trc20Contract(cntr, "Caller address", c);
        try {
            System.out.println("Total Supply: " + token.totalSupply());
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
```



### function balanceOf(address _owner)

Returns the account balance of another account with address `_owner`.

**PARAMS**

*1.  owner address*

The address of a token holder.

**RETURN**

The balance in a `Biginteger`.

**EXAMPLE**

```java
public void getBalanceOf() {
        Contract cntr = c.getContract("Contract address");
        Trc20Contract token = new Trc20Contract(cntr, "Caller address", c);
        try {
            System.out.println("Balance: " + token.balanceOf("Owner address"));
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
```



### function transfer(address _to, uint256 _value)

Transfers `_value` amount of tokens to address `_to`, and MUST fire the `Transfer` event.

**PARAMS**

*1. destAddr*

The receiver's address. 

*2. amount*

The transfer amount.

*3. memo*

The transaction memo.

*4. feeLimit*

The feelimit of a trigger call.

**RETURN**

A `TransactionReturn` object to show the transaction result.

**EXAMPLE**

```java
public void transfer() {
        Contract cntr = c.getContract("Contract address");
        Trc20Contract token = new Trc20Contract(cntr, "Caller address", c);
        try {
            // destnation address, amount, memo, feelimit
            System.out.println(token.transfer("To address", 10L, "go!", 100000000L));
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
```



### function transferFrom(address _from, address _to, uint256 _value)

Transfers `_value` amount of tokens from address `_from` to address `_to`, and MUST fire the `Transfer` event.

Normally works for withdraw workflow.

**PARAMS**

*1. fromAddr*

The address to withdraw from.

*2. destAddr*

The receiver's address. 

*3. amount*

The transfer amount.

*4. memo*

The transaction memo.

*5. feeLimit*

The feelimit of a trigger call.

**RETURN**

A `TransactionReturn` object to show the transaction result.

**EXAMPLE**

```java
public void transferFrom() {
        Contract cntr = c.getContract("Contract address");
        Trc20Contract token = new Trc20Contract(cntr, "Caller address", c);
        try {
            // destnation address, amount, memo, feelimit
            System.out.println(token.transferFrom("From Address", "To address", 10L, "go!", 100000000L));
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
```



### function approve(address _spender, uint256 _value)

Allows `_spender` to withdraw from your account multiple times, up to the `_value` amount. If this function is called again it overwrites the current allowance with `_value`.

**PARAMS**

*1. spender*

The spender's(withdrawer's) address.

*2. amount*

The allowance amount.

*3. memo*

The transaction memo.

*4. feeLimit*

The feelimit of a trigger call.

**RETURN**

A `TransactionReturn` object to show the transaction result.

**EXAMPLE**

```java
public void approve() {
        Contract cntr = c.getContract("Contract address");
        Trc20Contract token = new Trc20Contract(cntr, "Caller address", c);
        try {
            // destnation address, amount, memo, feelimit
            System.out.println(token.approve("Spender", 10L, "go!", 100000000L));
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
```



### function allowance(address _owner, address _spender)

Returns the amount which `_spender` is still allowed to withdraw from `_owner`.

**PARAMS**

*1. owner*

The owner's address.

*2. spender*

The spender's address.

**RETURN**

The approved withdrawal amount in a `Biginteger`.

**EXAMPLE**

```java
public void getAllowance() {
        Contract cntr = c.getContract("Contract address");
        Trc20Contract token = new Trc20Contract(cntr, "Caller address", c);
        try {
            System.out.println("Allowance: " + token.allowance("From address", "Spender"));
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
```



