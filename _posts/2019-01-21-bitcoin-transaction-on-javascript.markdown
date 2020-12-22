---
layout: post
title:  "Bitcoin transaction with bitcoinlib-js"
date:   2019-01-21
description: Bitcoin on javacript
---

<!-- Intro -->
### Intro
In this post I will cover step by step from creating a mnemonic to signing a transaction.

<!-- Library -->
### Dependencies
There are two main libraries bitcoinlib-js and bip 39.

<!-- Create mnemonic -->
### Generate mnemonic
We are going to use the function generateMemonic from bip39.

{% highlight javascript %}
import bip39 from 'bip39';

// mnemonic 12 words
var strength = 128;

// generate mnemonic
var mnemonic = bip39.generateMnemonic(strength);

console.log(mnemonic)
// => 'guilt balance hidden quiz obscure spend adult gas neutral snap hair arm'

{% endhighlight %}

<!-- Generate address -->

<!-- Fund address -->

<!-- Create raw transaction -->
