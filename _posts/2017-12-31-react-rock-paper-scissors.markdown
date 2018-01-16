---
layout: post
title:  "Solving rock paper scissors in a technical test environment"
date:   2017-12-31
description: A guideline to solve any technical test.
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
Having the target for the test done, we need to gather the requirements for the Rock, Paper and Scissors. If the challenge has requirements, you should follow them and mention them as you resolve the test. If the test doesn't come with, we need to create them. For this game I choose this requirements:

<!-- TODO add the requirements -->

<!-- TDD -->
Once we have the requirements, we have a basis for your unit tests. I am going to use most of the requirement and test those.

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

As you can see the unit tests will follow the requirements and are going to make use that independent the solution you take, the requirements will be taken into consideration.

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

{% endhighlight %}

The thirst solution is, in my opinion, the smaller and creative one. This solution is to create an array with an hierarchy, where the index X beat the index X + 1 or The initial index (0).
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
  import React from 'react';
  import { shallow, mount } from 'enzyme';
  import { shallowToJson } from 'enzyme-to-json';
  import toJson from 'enzyme-to-json';
  import App from './App';


  describe('<App />', () => {
    it('renders 1 <App /> component', () => {
      const component = shallow(<App />);
      expect(component).toHaveLength(1);
    })

    it('The Player picks rock, set the player move to rock', () => {
      const component = mount(<App />);
      const rockButton = component.find('button.rock-btn');
      expect(toJson(rockButton)).toMatchSnapshot();

      rockButton.simulate('click');
      expect(component.state().playerOneMove).toBe('rock');
    })

    it('The Player picks paper, set the player move to paper', () => {
      const component = mount(<App />);
      const paperButton = component.find('button.paper-btn');
      expect(toJson(paperButton)).toMatchSnapshot();

      paperButton.simulate('click');
      expect(component.state().playerOneMove).toBe('paper');
    })

    it('The Player picks scissors, set the player move to scissors', () => {
      const component = mount(<App />);
      const scissorsButton = component.find('button.scissors-btn');
      expect(toJson(scissorsButton)).toMatchSnapshot();

      scissorsButton.simulate('click');
      expect(component.state().playerOneMove).toBe('scissors');
    })

    it('The Ai picks a valid move after the player', () => {
      const component = mount(<App />);
      const availableMoves = ['rock', 'paper', 'scissors'];

      const scissorsButton = component.find('button.rock-btn');
      scissorsButton.simulate('click');

      expect(availableMoves).toContain(component.state().playerTwoMove);
    })

    it('The game ends and show the result after both players set the moves', () => {
      const component = mount(<App />);
      const scissorsButton = component.find('button.rock-btn');
      scissorsButton.simulate('click');
      expect(component.state().result).toBeTruthy();
    })
  })

{% endhighlight %}


<!-- Tips -->

<!-- Conclusion -->
In conclusion, when doing a technical test, always try to create a simple, scalable solution following all the required.
