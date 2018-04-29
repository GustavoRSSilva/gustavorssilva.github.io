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
4. Ethereum node, there many options, in this tutorial i am going to use GEth. (link to GETH)

Once you have them installed, we are ready to go the next step.

<!-- Testing your contract -->
### Getting the Contract ready to be deployed
Before deploying a contract into the test Network we should make sure that the contract has no errors. Once it is deployed there is no coming back. If you want to know about testing your contract you should ready this article about testing your contract. (link to my article about testing contracts).

In this tutorial i am going to use my contract from the previous post, my own ERC20 token Gtoken.

<!-- Gtoken -->
{% highlight solidity %}
pragma solidity ^0.4.19;


contract ERC20Interface {
    function totalSupply() public constant returns (uint);
    function balanceOf(address tokenOwner) public constant returns (uint balance);
    function allowance(address tokenOwner, address spender) public constant returns (uint remaining);
    function transfer(address to, uint tokens) public returns (bool success);
    function approve(address spender, uint tokens) public returns (bool success);
    function transferFrom(address from, address to, uint tokens) public returns (bool success);

    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}


contract Token is ERC20Interface {
    mapping (address => uint256) internal balances;
    mapping (address => mapping (address => uint256)) internal allowed;
    uint256 public totalAmount;

    function totalSupply() public constant returns (uint) {
      return totalAmount;
    }


    function transfer(address \_to, uint256 \_value) public returns (bool success) {
      if (balances[msg.sender] >= \_value && \_value > 0) {
        balances[msg.sender] -= \_value;
        balances[\_to] += \_value;
        Transfer(msg.sender, \_to, \_value);
        return true;
      } else {
        return false;
      }
    }


    function transferFrom(address \_from, address \_to, uint256 \_value) public returns (bool success) {
      if (balances[\_from] >= \_value && allowed[\_from][msg.sender] >= \_value && \_value > 0) {
        balances[\_to] += \_value;
        balances[\_from] -= \_value;
        allowed[\_from][msg.sender] -= \_value;
        Transfer(\_from, \_to, \_value);
        return true;
      } else {
        return false;
      }
    }

    function balanceOf(address \_owner) public constant returns (uint balance) {
      return balances[\_owner];
    }


    function approve(address \_spender, uint256 \_value) public returns (bool success) {
      allowed[msg.sender][\_spender] = \_value;
      Approval(msg.sender, \_spender, \_value);
      return true;
    }

    function allowance(address \_owner, address \_spender) public constant returns (uint256 remaining) {
      return allowed[\_owner][\_spender];
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

    /* Approves and then calls the receiving contract \*/
    function approveAndCall(address \_spender, uint256 \_value, bytes \_extraData) public returns (bool success) {
      allowed[msg.sender][\_spender] = \_value;
      Approval(msg.sender, \_spender, \_value);

      if (!\_spender.call(
        bytes4(bytes32(keccak256("receiveApproval(address,uint256,address,bytes)"))),
        msg.sender, \_value, this, \_extraData
      )) {
        revert();
      }

      return true;
    }
}
{% endhighlight %}

<!-- set the environment -->
### Getting Ether on the test network
To deploy a contract you need Ether to use it as Gas (link to gas). One of the main differences about the test networks is the path you take in order to get Ether, neither of them you use real Ethereum, and you can get it for free. In order to get some ether go to the Ropsten Network faucet page, www.faucet.Ropsten.com ? (link to the site) and request 18 Ether. All you need to do is share it on a social media. If you do not have any, you can send me a twit and i will send you some for free.

### Truffle
Once you have the Ether and the contract ready, the next step is to install truffle. If you are testing you contract you should already have it installed, if not, there is no problem Truffle is pretty easy to install, just follow the Installment on the Github page (link to github page).

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
To deploy a contract using truffle should not be a problem. Just use the command

- ``truffle deploy --network ropsten --reset``

<!-- Image of the command and the result -->

<!-- Use EtherScan to confirm that your contract was successfully deployed. -->
#### Use EtherScan to confirm that your contract was successfully deployed.
Once all the other steps are complet, we now need to verify if the contract is in the Network, just copy the address, go to (Link to ropsten etherscan) and search for it. There should an address with your contract information.
