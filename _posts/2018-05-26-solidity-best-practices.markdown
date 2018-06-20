 run bu---
layout: post
title:  "Simple guide to solidity best practices - Part 1"
date:   2018-05-21
description: A guideline for new developers
---

<!-- Introduction -->
## intro
I was going to write about some of the mistakes that I made during the first 6 months of developing contracts. Instead I am going write about some of the things I learnned? from those mistakes.

As usual i am going to go straight to the point, so here it goes:



<!-- Best practices -->
## Best practices

### Prepare for failure

<!-- decode this -->
Any non-trivial contract will have errors in it. Your code must, therefore, be able to respond to bugs and vulnerabilities gracefully.

Pause the contract when things are going wrong ('circuit breaker')
Manage the amount of money at risk (rate limiting, maximum usage)
Have an effective upgrade path for bugfixes and improvements

### Rollout carefully

It is always better to catch bugs before a full production release. - Test contracts thoroughly, and add tests whenever new attack vectors are discovered - Provide bug bounties starting from alpha testnet releases - Rollout in phases, with increasing usage and testing in each phase

### Use caution when making external calls

Calls to untrusted contracts can introduce several unexpected risks or errors. External calls may execute malicious code in that contract or any other contract that it depends upon. As such, every external call should be treated as a potential security risk. When it is not possible, or undesirable to remove external calls, use the recommendations in the rest of this section to minimize the danger.

### Use assert() and require() properly

In Solidity 0.4.10 assert() and require() were introduced. require(condition) is meant to be used for input validation, which should be done on any user input, and reverts if the condition is false. assert(condition) also reverts if the condition is false but should be used only for invariants: internal errors or to check if your contract has reached an invalid state. Following this paradigm allows formal analysis tools to verify that the invalid opcode can never be reached: meaning no invariants in the code are violated and that the code is formally verified.

### Avoid using storage


<!-- conclusion -->

#### Bibliography
https://github.com/ConsenSys/smart-contract-best-practices
https://consensys.github.io/smart-contract-best-practices/recommendations/
https://medium.com/@maurelian/beyond-smart-contract-best-practices-for-ux-and-interoperability-6d94d27c1e0f
https://medium.com/@yury.sannikov/solidity-smart-contract-audit-and-best-practices-2e80ce3edf68
