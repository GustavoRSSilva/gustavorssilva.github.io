---
layout: post
title:  "Deploy your first contract into Ropsten Network"
description: A guideline on how to deploy a contract into a test Network.
date:   2018-04-21
---
<!-- Intro -->
## Why Ropsten Network?
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
