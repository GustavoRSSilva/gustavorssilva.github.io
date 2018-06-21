---
layout: post
title:  "Solidity best practices - Part 1"
date:   2018-05-21
description: A guideline for new developers
---

<!-- Introduction -->
### intro

I was going to write about some of the mistakes that I made during the first 6 months of developing contracts fulltime but instead I am going write about the things i learned from those mistakes.
This post could also be called "things i wish I learned before starting work with the Ethereum".


<!-- Best practices -->
### Best practices

#### Prepare for failure

Even the most trivial contract can create problems if they are not tackled correctly.

{% highlight solidity %}
pragma solidity ^0.4.19;


contract math {
    function sub(uint a, uint b) returns(uint) {    
        return a - b;
    }    
}

{% endhighlight %}

This is a simple example of something that can go wrong. This code is fine when a is equal or bigger than b. The problem occurs when b is bigger. An unsigned integer is a integer that uses all of its bites to represent the value. A uint has 256 bites, the biggest value represented value is 1 - 2 ** 257.

sub(2, 1)
    //=>  1

sub(1, 2)
    //=> 1.157920892373162e+77

to solve this problem, use the Library <a href="https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/math/SafeMath.sol" target="\_blank">SafeMath</a> for all the operations. You can also use int instead, but it is not recommended to use negative numbers.

There are more examples that you need to be on the look for:

 * Race conditions
 * Reentracy
 * Transaction-Ordering Dependence
 * Timestamp Dependence
 * DoS


#### Avoid duing fancy stuff


#### Use assert() and require() properly

In Solidity 0.4.10 assert() and require() were introduced. require(condition) is meant to be used for input validation, which should be done on any user input, and reverts if the condition is false. assert(condition) also reverts if the condition is false but should be used only for invariants: internal errors or to check if your contract has reached an invalid state. Following this paradigm allows formal analysis tools to verify that the invalid opcode can never be reached: meaning no invariants in the code are violated and that the code is formally verified.

#### Avoid using storage

This one is more a personal opinion than a rule. There are two ways of storing information, by storing it directly in the memory, or by registering it in an event.

There are three main topics that you need to consider before making a decision, they normally collide between each other: Scalability, Centralization and Storage are the three main topics. The gas amount to execute as function will depend on those three factors. What I want to say, is that you don't always need to save everything on memory. let me give you two examples, one that i think you should save and one that i think you should not save.

The first example is clamming a reward, lets say that you are User claiming the reward. The contract should know if the User has claimed or not that reward, to avoid giving it two times. This is a valid example to save (normally by having a map (address => bool)).

The second case is celebrate the 1000th User that claimed the reward. Here you don't need to have an variable for that User. Just emit an event. Unless you need that for future, there is no need to have a variable just for that. It is unpractical and more expensive to use the storage.

#### Avoid iteration object

If you come from web development, one thing that you will try is to iterate an array of structs, you soon find out that <a href="https://blog.foam.space/exploring-ethereum-storage-costs-with-the-foam-developer-tools-96d84e1a06b5" target="\_blank">you cannot do that</a>. The hack that you will try to create is a map indexed by the Id and have an array of Ids. This way you can iterate an array and get the struct within the map. This in my opinion is wrong. Any operation that changes the state, will cost an exponential amount of money based on the size of the array, making it impossible to work with large amount of objects. It simply should be avoided.

<!-- conclusion -->

#### Bibliography

1. https://github.com/ConsenSys/smart-contract-best-practices
2. https://consensys.github.io/smart-contract-best-practices/recommendations/
3. https://medium.com/@maurelian/beyond-smart-contract-best-practices-for-ux-and-interoperability-6d94d27c1e0f
4. https://medium.com/@yury.sannikov/solidity-smart-contract-audit-and-best-practices-2e80ce3edf68
