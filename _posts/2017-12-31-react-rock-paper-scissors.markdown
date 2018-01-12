---
layout: post
title:  "Solving rock paper scissors with react"
date:   2017-12-31
---
<!-- Intro -->

<p class="intro"><span class="dropcap">O</span>nce a time ago a company based in London contacted me, they liked my curriculum and wanted to to complete a test in order to showcase my coding skills. They wanted me to create a Rock paper scissors game in angular.</p>

And so i <a href="https://github.com/GustavoRSSilva/rock-paper-scissors-the-game" target="\_blank">did</a>... and they never contacted me back.

There are many reasons why a company does not reply, this post is not about that, but looking back there are a lot more things that i could have done to showcase my coding skill. Let's take a look at what companies look for when developing technologies tests.

<!-- What companies look for when reviewing a test -->

#### What the company looks for when reviewing technical tests:
* The quality of the code;
* Did it followed the requirements;
* How much time did it took to finish the test;
* The technologies used

<!-- Requirements -->
Having the target for the test done, we need to gather the requirements for the Rock, Paper and Scissors. If the challenge has requirements, you should follow them and mention them as you resolve the test. If the test doesn't come with, we need to create them. For this game i choose this requirements:


<!-- TDD -->
Once we have the requirements, we have a basis for your unit tests. I am going to use most of the requirement and test those.

{% highlight javascript %}
<!-- TODO small preview of the unit tests -->
{% endhighlight %}

As you can see the unit tests will follow the requirements and are going to make use that independent the solution that you take to the test, the requirements will be respected.

<!-- Different solutions available -->
The App itself is a very simple game, it follow this sequence:
1. User chooses a move.
2. The computer chooses a move randomly.
3. The App resolves the game.
4. The App shows the result of the game.

In my opinion the only step here that you can showcase a different approach in the third step (resolving the game). The easier approach is to try to fit every possible move and set the user result:

{% highlight javascript %}
<!-- TODO add the code for the first step -->
{% endhighlight %}

the second step is to create an object that will show each move and his winning move (ex: The move Scissors and it beats paper.). In order for this step to work we need to first remove the draw result from a possible outcome.

{% highlight javascript %}
<!-- TODO add the code for the second step -->
{% endhighlight %}

The thirst solution is, in my opinion, the smaller and creative one. This solution is to create an array with an hierchi??, where the index X beat the index X + 1 or The initial index (0).
<!-- TODO graph image -->
This is a lightweight solution that is less scalable. Clearly the second solution is the better one. However i think that having a side solution can be very interesting to show creativity in solving tests. The end solution looks like this:

{% highlight javascript %}
<!-- TODO ad the code for the third step -->
{% endhighlight %}

<!-- Documentation -->
After having the tests and solution working we can create the documentation. Here i am going to talk about the requirements, the tests and the solution.
<!-- TODO link for the documentation -->


<!-- Resolution -->
Finally we have a functional resolved test.

{% highlight javascript %}
<!-- TODO Add the code for the solution -->
{% endhighlight %}

With the tests:

{% highlight javascript %}
<!-- TODO add the code for the tests -->
{% endhighlight %}


<!-- Tips -->

<!-- Conclusion -->
In conclusion, when doing a technical test, always try to create a simple, scalable solution following all the required.
