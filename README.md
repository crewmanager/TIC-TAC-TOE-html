# TIC-TAC-TOE-html

`main.mo`

```js
actor {
  type BoxInputData = {
    box0 : Text;
    box1 : Text;
    box2 : Text;
  };

  public query func changeTurn(turn : Text) : async Text {
    var result : Text = "";
    if (turn == "X") {
      result := "0";
    } else {
      result := "X";
    };
    return result;
  };

  public func checkWin(data : BoxInputData) : async Bool {
    if ((data.box0 == data.box1) and (data.box2 == data.box1) and (data.box0 != "")) {
      return true;
    };
    return false;
  };
};

```
`main.css`

```css
@import url("https://fonts.googleapis.com/css2?family=Roboto:ital@1&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Baloo+Bhaina+2&display=swap");

* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}

body {
  font-family: "Roboto", sans-serif;
  background-color: #f4f4f4;
  color: #333;
  line-height: 1.6;
}

nav {
  background-color: #333;
  color: #fff;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.4);
}

nav ul {
  list-style-type: none;
  display: flex;
}

nav ul li {
  margin-left: 20px;
}

nav ul li a {
  color: #fff;
  text-decoration: none;
  transition: color 0.3s ease;
}

nav ul li a:hover {
  color: #f76b1a;
}

.gameContainer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
  flex-wrap: wrap;
}

.container {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 10px;
}

.box {
  border: 2px solid #333;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 5vw;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.container div:hover {
  background-color: #e0e0e0;
  transform: scale(1.1);
}

.gameinfo {
  font-family: "Baloo Bhaina 2", cursive;
  font-size: 20px;
  max-width: 300px;
  margin: 0 auto; /* Centers the .gameinfo horizontally */
  display: flex;
  flex-direction: column;
  align-items: center; /* Centers the content horizontally in a flex container */
  text-align: center; /* Centers the text inside each child element */
}

.gameinfo span {
  color: #f76b1a;
  border: 3px solid #333;
  border-radius: 9px;
  padding: 5px 20px;
  display: inline-block;
  margin-top: 10px;
}

.gameinfo button {
  padding: 10px 15px;
  border-radius: 9px;
  background-color: #333;
  color: #fff;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.gameinfo button:hover {
  background-color: #f76b1a;
}

.gameinfo h1 {
  margin: 20px 0;
  font-size: 2rem;
}

@media screen and (max-width: 900px) {
  .gameContainer {
    flex-direction: column;
    align-items: center;
  }

  .container {
    grid-template-columns: repeat(3, 60px);
    grid-template-rows: repeat(3, 60px);
  }

  .gameinfo h1 {
    font-size: 1.5rem;
  }
}

```
`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="/main.css" />
    <title>Tic Tac Toe</title>
  </head>
  <body>
    <header>
      <nav id="navbar">
        <ul>
          <li>TicTacToeGame.com</li>
        </ul>
      </nav>
    </header>

    <div class="gameinfo">
      <h1>Tic Tac Toe Game</h1>
      <div id="status"></div>
      <div>
        <span class="info">Turns of X</span>
        <button id="reset">Reset</button>
      </div>
      <div class="imgbox">
        <img src="excit.gif" alt="" />
      </div>
    </div>

    <div class="gameContainer">
      <div id="container" class="container">
        <div class="box bt-0 bl-0"><span class="boxtext"></span></div>
        <div class="box bt-0"><span class="boxtext"></span></div>
        <div class="box bt-0 br-0"><span class="boxtext"></span></div>
        <div class="box bl-0"><span class="boxtext"></span></div>
        <div class="box"><span class="boxtext"></span></div>
        <div class="box br-0"><span class="boxtext"></span></div>
        <div class="box bb-0 bl-0"><span class="boxtext"></span></div>
        <div class="box bb-0"><span class="boxtext"></span></div>
        <div class="box bb-0 br-0"><span class="boxtext"></span></div>
      </div>
    </div>
  </body>
</html>

```
`index.js`

```js
import { todo_backend } from "../../declarations/todo_backend";

let turn = "X";
let gameOver = false;
let gameWorkingNow = false;
let status = document.getElementById("status");

// Function to change the turn
const changeTurn = async () => {
  let nextTurn = await todo_backend.changeTurn(turn);
  return nextTurn;
};

// Function to check for a win
const checkWin = async () => {
  gameWorkingNow = true;
  status.innerText = "Processing";
  let boxtext = document.getElementsByClassName("boxtext");
  let wins = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < wins.length; i++) {
    let data = {
      box0: boxtext[wins[i][0]].innerText,
      box1: boxtext[wins[i][1]].innerText,
      box2: boxtext[wins[i][2]].innerText,
    };
    let result = await todo_backend.checkWin(data);
    if (result) {
      document.querySelector(".info").innerText =
        "Player " + boxtext[wins[i][0]].innerText + " Won";
      gameOver = true;
      document
        .querySelector(".imgbox")
        .getElementsByTagName("img")[0].style.width = "200px";
      status.innerText = "";
      gameWorkingNow = false;
    }
  }

  status.innerText = "";
  gameWorkingNow = false;
};

// Game Logic
let boxes = document.getElementsByClassName("box");
Array.from(boxes).forEach((element) => {
  let boxtext = element.querySelector(".boxtext");
  element.addEventListener("click", async () => {
    if (boxtext.innerText === "" && !gameOver && !gameWorkingNow) {
      boxtext.innerText = turn;
      turn = await changeTurn();
      await checkWin();
      if (!gameOver) {
        document.getElementsByClassName("info")[0].innerText =
          "Turn for : " + turn;
      }
    }
  });
});

// Reset function
reset.addEventListener("click", (e) => {
  let boxtext = document.querySelectorAll(".boxtext");
  Array.from(boxtext).forEach((element) => {
    element.innerText = "";
  });
  turn = "X";
  document.getElementsByClassName("info")[0].innerText = "Turn for : " + turn;
  gameOver = false;
  document.querySelector(".imgbox").getElementsByTagName("img")[0].style.width =
    "0";
});

```
