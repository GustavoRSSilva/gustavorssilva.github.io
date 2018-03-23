---
layout: post
title:  "How to test your contact"
date:   2018-03-23
description: From unit testing to End to end.
---

<!-- Introduction -->
Once the contract is mined there is very little you can do to change it, so you have to be sure that everything works as intended.

There is no substitute for testing. In this tutorial I am going to talk about two types of tests, unit testing and e2e testing (End to End) applied to Ethereum Solidity.

I am going to create a <a href="https://en.wikipedia.org/wiki/ERC20" target="\_blank">erc20</a> token (Gtoken).
As with any feature, the first thing to do is to gather the requirements.

# requirements
1. The starting tokens amount is 1000;
2. When initiated the Owner address is award all the tokens;
3. It is possible to transfer tokens from one user to the other;

<!-- Unit test -->
## Unit testing

<a href="https://en.wikipedia.org/wiki/Unit_testing" target="\_blank">Unit testing</a> is a method where you test a individual unit of code. It focus on classes and classes' functions. The first two requirements are the best targets for unit testing, the 3th one is a end to end test.

{% highlight solidity %}
contract TestGtoken {
  Gtoken gtoken;

  function testInitialBalanceUsingDeployedContract() public {
    gtoken = Gtoken(DeployedAddresses.Gtoken());
    uint expected = 1000;
    Assert.equal(
      gtoken.balanceOf(tx.origin),
      expected,
      "Owner should have 1000 Gtoken initially"
    );
  }


  function testInitialBalanceWithNewGtoken() public {
    uint expected = 1000;

    /*The starting tokens amount is 1000*/
    Assert.equal(
      gtoken.totalSupply(),
      expected,
      "Owner should have 1000 Gtoken initially"
    );

    /*The amount of starting tokens is 1000*/
    Assert.equal(
      gtoken.balanceOf(tx.origin),
      expected,
      "Owner should have 1000 Gtoken initially"
    );
  }

  function beforeAll() public {
    gtoken = new Gtoken();
  }
}
{% endhighlight %}


<!-- E2E testing -->
## End to end testing (e2e)

<a href="https://www.techopedia.com/definition/7035/end-to-end-test" target="\_blank">End to end testing</a> is "used to test whether the flow of an application is performing as designed from start to finish"<a href="https://www.techopedia.com/definition/7035/end-to-end-test" target="\_blank">(source)</a>. the third requirement, to be possible to transfer tokens from one user to the other is is there with e2e testing. The End to end testing will use Javacript.

{% highlight javascript %}
var Gtoken = artifacts.require("./Gtoken.sol");

contract('Gtoken', function(accounts) {

  it("should put 1000 Gtoken in the first account", function() {
    return Gtoken.deployed().then(function(instance) {
      return instance.balanceOf.call(accounts[0]);
    }).then(function(balance) {
      assert.equal(
        balance.valueOf(),
        1000,
        "1000 wasn't in the first account");
    });
  });
  it("should send coin correctly", function() {
    var gToken;

    // Get initial balances of first and second account.
    var account_one = accounts[0];
    var account_two = accounts[1];

    var account_one_starting_balance;
    var account_two_starting_balance;
    var account_one_ending_balance;
    var account_two_ending_balance;

    var amount = 10;

    return Gtoken.deployed().then(function(instance) {
      gToken = instance;
      return gToken.balanceOf.call(account_one);
    }).then(function(balance) {
      account_one_starting_balance = balance.toNumber();
      return gToken.balanceOf.call(account_two);
    }).then(function(balance) {
      account_two_starting_balance = balance.toNumber();
      return gToken.transfer(
        account_two,
        amount,
        {from: account_one}
      );
    }).then(function() {
      return gToken.balanceOf.call(account_one);
    }).then(function(balance) {
      account_one_ending_balance = balance.toNumber();
      return gToken.balanceOf.call(account_two);
    }).then(function(balance) {
      account_two_ending_balance = balance.toNumber();

      assert.equal(
        account_one_ending_balance,
        account_one_starting_balance - amount,
        "Amount wasn't correctly taken from the sender"
      );
      assert.equal(
        account_two_ending_balance,
        account_two_starting_balance + amount,
        "Amount wasn't correctly sent to the receiver"
      );
    });
  });
});
{% endhighlight %}
