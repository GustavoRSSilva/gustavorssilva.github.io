---
layout: post
title:  "Deploy your first contract into Ropsten Network"
description: A guideline on how to deploy a contract into a test Network.
date:   2018-04-21
---
<!-- Intro -->
## Whats is the Ropsten Network?
Ropsten Newtwork is one three biggest Ethereum test Networks, the other two are Kinbery and Kovan.

<!-- Tools needed -->
### required Tools
in order to deploy a contract you are going to need the following tools:

1. The contract that is going to be used on the network.
2. Ether (Link to Ether), to deploy a contract you need gas (link to gas).
3. Truffle. (link to Truffle)
4. Ethereum node, there many options, in this tutorial i am going to a public one <a href="https://infura.io/" target="\_blank">Infura</a>.

<!-- Testing your contract -->
### Getting the Contract ready to be deployed
Before deploying a contract into the test Network we should make sure that the contract has no errors. Once it is deployed the only way to change is to deploy the contract again. This take time and Ether, this is why you should test your contract before deploying. If you want to know about testing your contract you should ready this article about <a href="http://www.gustavorssilva.com/blog/how-to-test-contracts/" target="\_blank">testing your contract</a>.

In this tutorial i am going to use my contract from the previous post, my own <a href="https://github.com/GustavoRSSilva/erc20-token" target="\_blank">ERC20 token Gtoken</a>.

<!-- Gtoken -->
{% highlight solidity %}
pragma solidity ^0.4.19;


contract ERC20Interface {
    function totalSupply() public constant returns (uint);
    function balanceOf(address tokenOwner) public constant
        returns (uint balance);
    function allowance(address tokenOwner, address spender)
        public constant returns (uint remaining);
    function transfer(address to, uint tokens) public
        returns (bool success);
    function approve(address spender, uint tokens) public
        returns (bool success);
    function transferFrom(address from, address to, uint tokens)
        public returns (bool success);

    event Transfer
        (address indexed from, address indexed to, uint tokens);
    event Approval (
        address indexed tokenOwner,
        address indexed spender,
        uint tokens
    );
}


contract Token is ERC20Interface {
    mapping (address => uint256) internal balances;
    mapping (address => mapping (address => uint256))
        internal allowed;
    uint256 public totalAmount;

    function totalSupply() public constant returns (uint) {
      return totalAmount;
    }


    function transfer(address to, uint256 value) public
        returns (bool success) {
            if (balances[msg.sender] >= value && value > 0) {
                balances[msg.sender] -= value;
                balances[to] += value;
                Transfer(msg.sender, to, value);
                return true;
            } else {
                return false;
            }
        }


    function transferFrom(address from, address to, uint256 value)
    public returns (bool success) {
            if (
                balances[from] >= value &&
                allowed[from][msg.sender] >= value &&
                value > 0
            ) {
                balances[to] += value;
                balances[from] -= value;
                allowed[from][msg.sender] -= value;
                Transfer(from, to, value);
                return true;
            } else {
                return false;
            }
        }

    function balanceOf(address owner) public constant
        returns (uint balance) {
            return balances[owner];
        }


    function approve(address spender, uint256 value) public
        returns (bool success) {
            allowed[msg.sender][spender] = value;
            Approval(msg.sender, spender, value);
            return true;
        }

    function allowance(address owner, address spender)
        public constant returns (uint256 remaining) {
            return allowed[owner][spender];
        }
}


contract Gtoken is Token {
    function Gtoken() public {
        balances[tx.origin] = 1000;
        totalAmount = 1000;             // total supply of token
        name = "Gtoken";               // name of token
        decimals = 0;                 // amount of decimals
        symbol = "GTKN";              // symbol of token
    }

    function () public payable {
        //if ether is sent to this address, send it back.
        revert();
    }

    string public name;
    uint8 public decimals;
    string public symbol;

    function approveAndCal
        (address spender, uint256 value, bytes extraData)
        public
        returns (bool success) {
            allowed[msg.sender][spender] = value;
            Approval(msg.sender, spender, value);

            if (!spender.call(
                bytes4(bytes32(keccak256(
                    "receiveApproval(address,uint256,address,bytes)"
                ))),
                msg.sender, value, this, extraData
            )) {
                revert();
            }

            return true;
        }
}
{% endhighlight %}

<!-- set the environment -->
### Getting Ether on the test network
To deploy a contract you need Ether to use it as Gas (link to gas). One of the main differences about the test networks is the path you take in order to get Ether, neither of them you use real Ethereum, and you can get it for free. In order to get some ether go to the Ropsten Network faucet page, <a href="http://faucet.ropsten.be:3001/" target="\_blank">http://faucet.ropsten.be:3001/</a> and request 1 Ether. Now you hvae 1 Ether on your account.

### Truffle
Once you have the Ether and the contract ready, the next step is to install truffle. If you are testing you contract you should already have it installed, if not, there is no problem Truffle is pretty easy to install, just follow the Installment on the <a href="https://github.com/trufflesuite/truffle" target="\_blank">Github page</a>.

### Ethereum node
The last thing missing is the Ethereum node. In order to deploy a contract, you need to be connected to the blockchain. You are also going to need an account there with Ether in order to deploy the contract. In this post i am going to use GETH (link to Geth), The Ethereum Node written in GoLang. The installation should be straight forward.

### Deployment
Once you have all the tools isntalled its time to go the deployment. just to make things clear I am going to list all the steps you need:

1. Start GETH.
2. Connect your private account to GETH.
3. Use truffle to deploy the contract and all the dependencies.
4. Use EtherScan to confirm that your contract was successfully deployed.

<!-- Start Geth -->
<!-- connect yout private account to Geth -->
<!-- Use truffle to deploy the contract and all the dependencies. -->
#### Use truffle to deploy the contract and all the dependencies.

To deploy a contract using truffle should not be a problem. Just use the command</br>
``` truffle deploy --network ropsten --reset ```

<!-- Image of the command and the result -->

<!-- Use EtherScan to confirm that your contract was successfully deployed. -->
#### Use EtherScan to confirm that your contract was successfully deployed.
Once all the other steps are complet, we now need to verify if the contract is in the Network, just copy the address, go to (Link to ropsten etherscan) and search for it. There should an address with your contract information.
