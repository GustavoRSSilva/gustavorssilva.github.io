---
layout: post
title:  "Deploy your first contract into Ropsten Network"
description: A guideline on how to deploy a contract into a test Network.
date:   2018-04-21
---
<!-- Intro -->
## Why deploy on the test network?
before you deploy you contract into the Mainnet, you should test it, failling to do so, it is very expensive, so you need a solution to test your contract before deploying it. Ropsten is the perfect network to do it, being of th three biggest Test networks, it is fast, due to the use proof of authority instead of Ethereum Proof of work and it is easy to earn Ether. It this tutorial We are going to a contract in Ropsten Network.

<!-- Tools needed -->
### required Tools
in order to deploy a contract you are going to need the following tools:

1. The contract;
2. <a href="https://www.ethereum.org/ether" target="\_blank">Ether</a>, to convert into <a href="https://ethgasstation.info/" target="\_blank">gas</a>;
3. <a href="http://truffleframework.com/" target="\_blank">Truffle</a>;
4. Ethereum node, <a href="https://infura.io/" target="\_blank">Infura</a>.

<!-- Testing your contract -->
### Getting the Contract ready to be deployed
Before deploying a contract into the test Network we should make sure that the contract has no errors. Once it is deployed the only way to change is to deploy the contract again. This take time and Ether, so you should test your contract before deploying. If you want to know about testing your contract you should read this article about <a href="http://www.gustavorssilva.com/blog/how-to-test-contracts/" target="\_blank">testing your contract</a>.

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
Before deploying your contract you need <a href="https://ethgasstation.info/" target="\_blank">Gas</a>. One of the main differences between test networks is how you get Ether, neither of them you use real Ethereum, and you can get it for free. In order to get some Ether go to the Ropsten Network faucet page, <a href="http://faucet.ropsten.be:3001/" target="\_blank">http://faucet.ropsten.be:3001/</a> and request 1 Ether. Now you have 1 Ether on your account.

### Truffle
Once you have the Ether and the contract ready, the next step is to install truffle. If you are testing you contract you should already have it installed, if not, Truffle is pretty easy to install, just follow the Installment on the <a href="https://github.com/trufflesuite/truffle" target="\_blank">Github page</a>.


{% highlight javascript %}
var HDWalletProvider = require("truffle-hdwallet-provider");

var infuraToken = 'gR45rfdSDEWWQe';
var mnemonic =
    'today blue egg air living keys black water pass wheel nerd to twelve';

module.exports = {
    ropsten: {
      provider: function() {
        return new HDWalletProvider(
            mnemonic,
            `https://ropsten.infura.io/${infuraToken}`
        )
      },
      network_id: "3",
      gas: 4700000
    },
  }
};

{% endhighlight %}

### Ethereum node
The last thing missing is the Ethereum node. In order to deploy a contract, you need to be connected to the Blockchain. I am going to use the public node  <a href="https://infura.io/" target="\_blank">Infura</a> public node. Infura is very easy to use, just login into their and and you will recieve a key. You need that token to connect to the network.

If you do not use a public network, you can use a private local node. Geth or Tmux are the best options.

### Deployment
Once you have all the tools installed it is time for the deployment. Just to make things clear I am going to list all the steps you need:

1. Register in Infura.
2. Set your mnemonic and infura key.
3. Use truffle to deploy the contract and all the dependencies.
4. Use EtherScan to confirm that your contract was successfully deployed.


<!-- Start Geth -->
<!-- connect yout private account to Geth -->
<!-- Use truffle to deploy the contract and all the dependencies. -->
#### Use truffle to deploy the contract and all the dependencies.
To deploy a contract using truffle should not be a problem. Just use the command

``` truffle deploy --network ropsten --reset ```

<figure>
	<img src="{{ '/assets/img/succDeployment.png' | prepend: site.baseurl }}" alt="">
	<figcaption>Fig1. - Successful deployment</figcaption>
</figure>

<!-- Use EtherScan to confirm that your contract was successfully deployed. -->
#### Use EtherScan to confirm that your contract was successfully deployed.

Once all the other steps are complet, we now need to verify if the contract is in the Network, just copy the address, go to <a href="https://ropsten.etherscan.io/" target="\_blank">https://ropsten.etherscan.io/</a> and search for it. There should an address with your contract information. Your contract should be there <a href="https://ropsten.etherscan.io/address/0x9fa6566fe4c4af44c26ff0dba146d9aed7c63c9f" target="\_blank" >https://ropsten.etherscan.io/address/0x9fa6566fe4c4af44c26ff0dba146d9aed7c63c9f</a>
