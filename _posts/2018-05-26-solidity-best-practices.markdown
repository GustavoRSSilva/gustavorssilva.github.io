---
layout: post
title:  "[WIP] Simple guide to solidity best practices"
date:   2018-05-21
description: A guideline for new developers
---

<!-- Introduction -->
Developing on the blockchain it is still in its infancy.In this post i am going to point out some of the best practices when developing a contract in Solidity.

<!-- Best practices -->
## Best practices

### Prepare for failure

<!-- decode this -->
Any non-trivial contract will have errors in it. Your code must, therefore, be able to respond to bugs and vulnerabilities gracefully.

Pause the contract when things are going wrong ('circuit breaker')
Manage the amount of money at risk (rate limiting, maximum usage)
Have an effective upgrade path for bugfixes and improvements

<!-- conclusion -->

#### Bibliography
https://github.com/ConsenSys/smart-contract-best-practices
https://consensys.github.io/smart-contract-best-practices/recommendations/
https://medium.com/@maurelian/beyond-smart-contract-best-practices-for-ux-and-interoperability-6d94d27c1e0f
https://medium.com/@yury.sannikov/solidity-smart-contract-audit-and-best-practices-2e80ce3edf68
