---
layout: post
title:  "A small introduction to Ethereum DApps"
date:   2018-02-21
description: From Web APPs to DApps
---

<p class="intro">Blockchain is on vogue, many companies are now discovering how powerfull a descentrelized approach can improve <a href="https://techcrunch.com/2017/10/16/ibm-cross-border-payments-Blockchain/" target="\_blank">their product</a>. As more companies discover, more demand for Blockchain developers there will be. In this article in am going to try to answer some question about DApps and how they integrate with the Blockchain.</p>

#### What is a DApp ?
DApp stands for decentralized application. By definition a Application is program that satifies a group of requirements. There are many different types of applications ranging from gaming, learning, ordering food to getting a ride. A normal web application by nature is located in one or more predefined servers. Those severs are a located in one or more spaces, they are centralized. The owner has control of the information and can change it if needed. A DApp on the other hand is an application a located in the Blockchain, once it is deployed, the owner has a limited control over it.
A DApp can also be classified as a group of contracts.

#### What language does a Ethereum DApp support ?
You have a wide range of languages available to develop a DApp, basically every language that can be compiled Ethereum. While the most common is <a href="https://github.com/ethereum/solidityn" target="\_blank">solidity</a>, you can use <a href="https://media.consensys.net/an-introduction-to-lll-for-ethereum-smart-contract-development-e26e38ea6c23" target="\_blank">LLL</a>, <a href="https://github.com/ethereum/wiki/wiki/Serpent" target="\_blank"Serpent</a>, <a href="https://github.com/ewasm/design" target="\_blank">eWasm</a> and many others.

#### Where is the code stored ?
In a classic Web API the code is stored in one or more servers, while in a DApp the code is stored in the block, once the code its mined, it is propagated throw the Blockchain.

#### Where is the data stored ?
In DApps, the data is stored in the memory (storage). Works a little bit like memory storage in Javascript. Once again this is different from an API, where the data can be stored in one or more databases or files.

#### What implications does it have ?
The fact that DApps are stored in the block, once it is mined, it is impossible to change the main code. It implies a heavy assertment in the code base where you don't want to have bugs that lock people's assets. We can all agree that Ethereum had enough of that, after all there is a reason we have Ethereum and Ethereum classic (link to the error in the code).

However you can still change the configurations, otherwise a contract that has a pre assigned gas to a action would become unavailable with all the fluctuation that exist in the gas necessary to invoke an transaction.

#### How to deploy a DApp ?
A DApp can be classified as a group of contracts, and in order to fully work the contracts need to be available in the Blockchain. Sending an contract is the same as sending a transaction. Once you assign a gas value  that contract and send it, the miners will try to mine it to create a new block.

As for the deployment itself you want to use Ganache (formerly TestRPC), and you will want to use the Truffle framework.

#### Where can I see a list of available DApps ?
You can find the list of available DApps on <a href="https://www.stateofthedapps.com/" target="\_blank">stateofthedapps.com</a>

Hopefully this brief introduction will get you interested in starting learning more about DApps. If you want to start developing on Ethereum I would recommend for you to start with <a href="https://cryptozombies.io/" target="\_blank">CryptoZombies</a>, a funny and interactive game.
