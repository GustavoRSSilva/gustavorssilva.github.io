---
layout: post
title:  "Solving rock paper scissors with react"
date:   2017-12-31
---

<p class="intro"><span class="dropcap">O</span>nce a time ago a company based in London contacted me, they liked my curriculum and wanted to to complete a test in order to showcase my coding skills. They wanted me to create a Rock paper scissors game in angular. </p>

Jekyll also offers powerful support for code snippets:

{% highlight javascript %}
import React, { Component } from 'react';
import 'font-awesome/css/font-awesome.min.css';
import './App.css';
import Carousel from './components/Carousel';
import { ROCK, PAPER, SCISSORS, AVAILABLE_MOVES } from './constants';

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      playerOneMove: null,
      playerTwoMove: null,
      errorLog: null,
      result: null,
    };
  }

  resetGame() {
    this.setState({
      playerOneMove: null,
      playerTwoMove: null,
      errorLog: null,
      result: null,
    });
  }

  setMoves(playerOneMove, playerTwoMove = null) {
    const availableMoves = AVAILABLE_MOVES;

    //If the Player one move is not valid, we show a error message
    if(!(availableMoves.indexOf(playerOneMove) > -1)) {
      return this.setState({ errorLog: 'Invalid move' });
    }

    //If the Player two move is not valid, we show a error message
    if(playerTwoMove !== null && !(availableMoves.indexOf(playerTwoMove) > -1)) {
      return this.setState({ errorLog: 'Invalid move' });
    }

    //In order to not override a function parameter, we create an auxiliary variable
    let _playerTwoMove = playerTwoMove;

    //If player two move is null, we set it up
    if(_playerTwoMove === null) {
      _playerTwoMove = availableMoves[Math.floor(Math.random() * availableMoves.length)];
    }

    this.setState(
      { playerOneMove, playerTwoMove: _playerTwoMove },
      () => this.setGameResult()
    );
  }

  setGameResult() {
    const { playerOneMove, playerTwoMove } = this.state;

    if(!playerOneMove || !playerTwoMove) {
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

  renderResult() {
    return (
      <div className="App">
        <header>
          <h1 className="App-result-title">{this.state.result}</h1>
          <h2>
            <p>You picked <i className={`fa fa-hand-${this.state.playerOneMove}-o large`} /></p>
            <p>Vs</p>
            <p>Your opponent picked <i className={`fa fa-hand-${this.state.playerTwoMove}-o large`} /></p>
          </h2>
        </header>
        <button
          className="restart-btn"
          onClick={() => this.resetGame()}
        >
          Restart Game
        </button>
      </div>
    );
  }

  render() {
    // If the game over show the result and the restart button
    if(this.state.result) {
      return this.renderResult();
    }

    return (
      <div className="App">
        <header className="App-header">
          <h1 className="App-title">Rock, paper, scissors</h1>
          <h2>vs AI</h2>
        </header>
        <p>
          Please choose a move.
        </p>
        <div id="player-one-container">
          <button
            className="rock-btn yoo"
            onClick={() => this.setMoves(ROCK)}
          >
            <i className="fa fa-hand-rock-o large"></i>
          </button>
          <button
            className="scissors-btn"
            onClick={() => this.setMoves(SCISSORS)}
          >
            <i className="fa fa-hand-scissors-o large"></i>
          </button>
          <button
            className="paper-btn"
            onClick={() => this.setMoves(PAPER)}
          >
            <i className="fa fa-hand-paper-o large"></i>
          </button>
        </div>
        <p>
          vs
        </p>
        <Carousel
          options={AVAILABLE_MOVES.map((move) => (<i className={`fa fa-hand-${move}-o large`} />))}
        />
        <p className="App-error-log">
          {this.state.errorLog}
        </p>
        <p className="app-footer">
          For more information about the project, see the <a href="" target="_blank">blog post</a>.
        </p>
      </div>
    );
  }
}

export default App;
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh].

[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com
