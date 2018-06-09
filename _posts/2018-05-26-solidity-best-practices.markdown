---
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

<!-- conclusion -->

#### Bibliography
https://github.com/ConsenSys/smart-contract-best-practices
https://consensys.github.io/smart-contract-best-practices/recommendations/
https://medium.com/@maurelian/beyond-smart-contract-best-practices-for-ux-and-interoperability-6d94d27c1e0f
https://medium.com/@yury.sannikov/solidity-smart-contract-audit-and-best-practices-2e80ce3edf68
