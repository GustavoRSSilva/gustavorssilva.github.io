---
layout: post
title:  "Solving rock paper scissors in a technical test environment"
date:   2018-01-21
description: A guideline to solve any technical test.
---
<!-- Intro -->

<p class="intro"><span class="dropcap">O</span>nce a time ago a company based in London contacted me, they liked my curriculum and wanted to to complete a test in order to showcase my coding skills. They wanted me to create a Rock paper scissors game in angular.</p>

And so i <a href="https://github.com/GustavoRSSilva/rock-paper-scissors-the-game" target="\_blank">did</a>... and they never contacted me back.

There are many reasons why a company does not reply, this post is not about that, but looking back there are a lot more things that I could have done to showcase my coding skills. Let's take a look at what companies look for when developing technologies tests.

<!-- What companies look for when reviewing a test -->

#### What the company looks for when reviewing technical tests:
* The quality of the code;
* Did it followed the requirements;
* How much time did it took to finish the test;
* The technologies used

<!-- Requirements -->
In order to start we need to gather the requirements for the Rock, Paper and Scissors. If the challenge comes with requirements, you should follow them and mention them as you resolve the test. If the test doesn't come with, we need to create them. For this game I choose the following requirements:

* The player one chooses one of the three available moves (rock, paper, scissors);
* After the player one move, the AI picks one of the three available moves at random;
* Once both moves are picked, the App resolves the game and shows the result;

<!-- TDD -->
Now we have a basis for our unit tests. I am going to use the requirements in the tests.

{% highlight javascript %}

describe('<App />', () => {
  it('renders 1 <App /> component', () => { ... })

  it('The Player picks rock, set the player move to rock', () => { ... })

  it('The Player picks paper, set the player move to paper', () => { ... })

  it('The Player picks scissors, set the player move to scissors', () => { ... })

  it('The Ai picks a valid move after the player', () => { ... })

  it('The game ends and show the result after both players set the moves', () => { ... })
})

{% endhighlight %}

As you can see the unit tests will follow the requirements and are going to be taken into consideration, independently the solution you take.

<!-- Different solutions available -->

##### The App itself is a very simple game, it follow this sequence:
1. User chooses a move;
2. The computer chooses a move randomly;
3. The App resolves the game.
4. The App shows the result of the game.

In my opinion the only step here that you can showcase a different approach in the third step (resolving the game). The easier approach is to try to fit every possible move and set the user result:

{% highlight javascript %}


class App extends Component {
  //...

  validMove(target) {
    return target && moves[target];
  }  

  setGameResult() {
    const { playerOneMove, playerTwoMove } = this.state;

    if(!this.validMove(playerOneMove) || !this.validMove(playerTwoMove)) {
      return this.setState({ errorLog: 'Invalid operation!' });
    }

    //Player one wins?
    if(
      (playerOneMove === ROCK && playerTwoMove === SCISSORS) ||
      (playerOneMove === PAPER && playerTwoMove === ROCK) ||
      (playerOneMove === SCISSORS && playerTwoMove === PAPER)
    ) {
      return this.setState({ result: 'You win!' });
    }

    //Player one loses?
    if(
      (playerOneMove === SCISSORS && playerTwoMove === ROCK) ||
      (playerOneMove === ROCK && playerTwoMove === PAPER) ||
      (playerOneMove === PAPER && playerTwoMove === SCISSORS)
    ) {
      return this.setState({ result: 'You lose!' });
    }

    return this.setState({ result: 'It\'s a draw' });
  }

  //...
{% endhighlight %}

the second step is to create an object that will show each move and his winning move (ex: The move Scissors and it beats paper.). In order for this step to work we need to first remove the draw result from a possible outcome.

{% highlight javascript %}
  const moves = {
    rock: {
      beats: 'scissors',
    },
    scissors: {
      beats: 'paper',
    },
    paper: {
      beats: 'rock',
    }
  }

  class App extends Component {
    //...

    validMove(target) {
      return target && moves[target];
    }  

    setGameResult() {
      const { playerOneMove, playerTwoMove } = this.state;

      if(!this.validMove(playerOneMove) || !this.validMove(playerTwoMove)) {
        return this.setState({ errorLog: 'Invalid operation!' });
      }

      switch (playerOneMove) {
        case playerTwoMove:
          return this.setState({ result: 'It\'s a draw' });

        case moves[playerTwoMove].beats:
          return this.setState({ result: 'You lose!' });

        default:
          return this.setState({ result: 'You win!' });
      }
    }

    //...
  }
{% endhighlight %}

The thirst solution is, in my opinion, the smaller and creative one. This solution is to create an array with an hierarchy, where the index X beat the index X + 1 or The initial index (0).

<figure>
	<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/67/Rock-paper-scissors.svg/1200px-Rock-paper-scissors.svg.png" alt="rock_paper_scissors">
	<figcaption>Fig1. - Rock, Paper and Scissors hierarchy</figcaption>
</figure>

This is a lightweight solution that is less scalable. Clearly the second solution is the better one. However I think that having a side solution can be very interesting to display creativity. The end solution looks like this:

{% highlight javascript %}
  const moves = ['rock', 'scissors', 'paper'];

  class App extends Component {
    //...

    validMove(target) {
      return target && moves[target];
    }  

    setGameResult() {
      const { playerOneMove, playerTwoMove } = this.state;

      if(!this.validMove(playerOneMove) || !this.validMove(playerTwoMove)) {
        return this.setState({ errorLog: 'Invalid operation!' });
      }

      if (playerOneMove === playerTwoMove) {
        return this.setState({ result: 'It\'s a draw' });
      }

      return ( moves[moves.indexOf(playerOneMove) + 1] || moves[0]) ===  playerTwoMove ? this.setState({ result: 'You win!' }) : this.setState({ result: 'You lose!' });
    }

    //...
  }


{% endhighlight %}

<!-- Documentation -->
After having the tests and solution the only thing missing is the Documentation. The documentation should aim to teach the user about the project and how to run it. <a href="https://github.com/GustavoRSSilva/react-rock-paper-scissors-vs-ai/wiki"> Here I am going to talk about the requirements, the tests and the solution.</a>


<!-- Conclusion -->
In conclusion, when doing a technical test, always try to create a simple, scalable solution following all the required.

The following test can be consulted <a href="https://github.com/GustavoRSSilva/react-rock-paper-scissors-vs-ai">here</a>.
