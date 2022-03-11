![Lime green geometric design. Get in touch to learn more!](./assets/banner.png)

[www.immanuelcurrah.com](https://www.immanuelcurrah.com/)

# Mission Statement

As a previous zen monk with over 15 years of experience in construction, I bring a diverse and user-centered approach to software engineering. My skills and experience allow me to seamlessly take feedback and improve my code, while also asking questions when needed. Paired with a love of technology, I also thrive on working in fast paced environments to build and improve applications.

# What I love to work with

![]()<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" width="50" />
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" width="50" />
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/express/express-original.svg" width="50" />
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mongodb/mongodb-original-wordmark.svg" width="50" />
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/postgresql/postgresql-original.svg" width="50"/>
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/ruby/ruby-plain-wordmark.svg" width="50" />
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/rails/rails-plain.svg" width="50" />
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original.svg" width="50" />
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original.svg" width="50" />
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/bootstrap/bootstrap-plain.svg" width="50" />
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/figma/figma-original.svg" width="50" />
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" width="50" />
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/npm/npm-original-wordmark.svg" width="50"/>
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/heroku/heroku-plain.svg" width="50"/>
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/vscode/vscode-original.svg" width="50" />

# How to reach me

[![Gmail contact badge](./assets/gmail.png)](mailto:immanueli.currah@gmail.com)
<a href="https://www.linkedin.com/in/immanuelcurrah/"><img src="./assets/linkedin.png" width="75" ></a>

# Code Snippet

This is a custom hook I wrote for my second project with General Assembly. It is used for movement commands for different styled mazes. Each map has a unique layout. So this hook was made to be used dynamically with all of them. I know it's a bit long, but I am only putting it here because it is the code I am most proud of writing.

```
import { useState } from "react";
import { useGetHighScoresUpdate } from "./useHighScoresUpdate";
import { useNavigate } from "react-router-dom";

export const useMap = (map) => {
  const [grid, setGrid] = useState(map);
  const [toggleMap, setToggleMap] = useState(false);
  const [startGame, setStartGame] = useState(false);
  const [stopGame, setStopGame] = useState(false);
  const [lastSecond, setLastSecond] = useState(0);
  const [lastMinute, setLastMinute] = useState(0);
  const [currentMazeName, setCurrentMazeName] = useState("");
  const [score, setScore] = useState(0);

  let trueScore = score + lastSecond + lastMinute * 60;

  const { timeHandler } = useGetHighScoresUpdate(stopGame);
  const navigate = useNavigate();

  const recordTimerHandler = (seconds, minutes) => {
    if (stopGame === false) {
      setLastSecond(seconds);
      setLastMinute(minutes);
      timeHandler(lastSecond, lastMinute, currentMazeName, trueScore);
    } else if (stopGame === true) {
      console.log("ps. That was slow af");
      return;
    }
  };

  //below is how each step of the function works and what its checking for. they all follow the same pattern. Except for move right, see below
  // MOVE UP
  const moveUp = () => {
    let length = grid.length;
    let loopRow = 0;
    let loopCol = 0;

    for (let i = 0; i < length; i++) {
      for (let j = 0; j < length; j++) {
        if (
          grid[i][j] === 3 ||
          grid[i][j] === 6 ||
          grid[i][j] === 7 ||
          grid[i][j] === 8
        ) {
          loopRow = i;
          loopCol = j;
        }
      }
    }
    // checks for walls
    if (grid[loopRow][loopCol - 1] === 2 || grid[loopRow][loopCol - 1] === 9) {
      return;
    } else if (
      //checks if moved into a bat
      grid[loopRow][loopCol - 1] === 10 ||
      grid[loopRow][loopCol - 1] === 11 ||
      grid[loopRow][loopCol - 1] === 13
    ) {
      window.location.reload(false);
      console.log("you lost");
      //checks for a gem
    } else if (grid[loopRow][loopCol - 1] === 12) {
      const newGrid = grid;
      grid[loopRow][loopCol - 1] = 3;
      grid[loopRow][loopCol] = 1;
      setGrid(newGrid);
      setScore((prevScore) => prevScore - 100);
      //regular movement
    } else {
      const newGrid = grid;
      newGrid[loopRow][loopCol - 1] = 3;
      newGrid[loopRow][loopCol] = 1;
      setGrid(newGrid);
    }
    //toggles the map and then adds points based on steps
    setToggleMap((prevToggle) => !prevToggle);
    setScore((prevScore) => prevScore + 10);
  };

  //MOVE DOWN
  const moveDown = () => {
    let length = grid.length;
    let loopRow = 0;
    let loopCol = 0;

    for (let i = 0; i < length; i++) {
      for (let j = 0; j < length; j++) {
        if (
          grid[i][j] === 3 ||
          grid[i][j] === 6 ||
          grid[i][j] === 7 ||
          grid[i][j] === 8
        ) {
          loopRow = i;
          loopCol = j;
        }
      }
    }
    if (grid[loopRow][loopCol + 1] === 2 || grid[loopRow][loopCol + 1] === 9) {
      return;
    } else if (
      grid[loopRow][loopCol + 1] === 10 ||
      grid[loopRow][loopCol + 1] === 11 ||
      grid[loopRow][loopCol + 1] === 13
    ) {
      window.location.reload(false);
      console.log("you lost");
    } else if (grid[loopRow][loopCol + 1] === 12) {
      const newGrid = grid;
      grid[loopRow][loopCol + 1] = 6;
      grid[loopRow][loopCol] = 1;
      setGrid(newGrid);
      setScore((prevScore) => prevScore - 200);
    } else {
      const newGrid = grid;
      grid[loopRow][loopCol + 1] = 6;
      grid[loopRow][loopCol] = 1;
      setGrid(newGrid);
    }
    setToggleMap((prevToggle) => !prevToggle);
    setScore((prevScore) => prevScore + 10);
  };

  //MOVE RIGHT
  const moveRight = () => {
    let length = grid.length;
    let loopRow = 0;
    let loopCol = 0;

    for (let i = 0; i < length; i++) {
      for (let j = 0; j < length; j++) {
        if (
          grid[i][j] === 3 ||
          grid[i][j] === 6 ||
          grid[i][j] === 7 ||
          grid[i][j] === 8
        ) {
          loopRow = i;
          loopCol = j;
        }
      }
    }
    //checks for walls
    if (grid[loopRow + 1][loopCol] === 2 || grid[loopRow + 1][loopCol] === 9) {
      return;
    } else if (
      //checks for bats
      grid[loopRow + 1][loopCol] === 10 ||
      grid[loopRow + 1][loopCol] === 11 ||
      grid[loopRow + 1][loopCol] === 13
    ) {
      window.location.reload(false);
      //checks for gems
    } else if (grid[loopRow + 1][loopCol] === 12) {
      const newGrid = grid;
      grid[loopRow + 1][loopCol] = 7;
      grid[loopRow][loopCol] = 1;
      setGrid(newGrid);
      setScore((prevScore) => prevScore - 200);
      //checks for the chest and end of game
    } else if (grid[loopRow + 1][loopCol] === 4) {
      grid[loopRow + 1][loopCol] = 5;
      grid[loopRow][loopCol] = 7;
      setStopGame(true);
      setTimeout(() => {
        navigate("/highscores");
        window.location.reload(false);
      }, 500);
    } else {
      //regular movement
      const newGrid = grid;
      grid[loopRow + 1][loopCol] = 7;
      grid[loopRow][loopCol] = 1;
      setGrid(newGrid);
    }
    //toggles map, points, and start of game
    setToggleMap((prevToggle) => !prevToggle);
    setScore((prevScore) => prevScore + 10);
    setStartGame(true);
  };

  //MOVE LEFT
  const moveLeft = () => {
    let length = grid.length;
    let loopRow = 0;
    let loopCol = 0;

    for (let i = 0; i < length; i++) {
      for (let j = 0; j < length; j++) {
        if (
          grid[i][j] === 3 ||
          grid[i][j] === 6 ||
          grid[i][j] === 7 ||
          grid[i][j] === 8
        ) {
          loopRow = i;
          loopCol = j;
        }
      }
    }
    if (grid[loopRow - 1][loopCol] === 2 || grid[loopRow - 1][loopCol] === 9) {
      return;
    } else if (
      grid[loopRow - 1][loopCol] === 10 ||
      grid[loopRow - 1][loopCol] === 11 ||
      grid[loopRow - 1][loopCol] === 13
    ) {
      window.location.reload(false);
      console.log("you lost");
    } else if (grid[loopRow - 1][loopCol] === 12) {
      const newGrid = grid;
      grid[loopRow - 1][loopCol] = 8;
      grid[loopRow][loopCol] = 1;
      setGrid(newGrid);
      setScore((prevScore) => prevScore - 200);
    } else {
      const newGrid = grid;
      grid[loopRow - 1][loopCol] = 8;
      grid[loopRow][loopCol] = 1;
      setGrid(newGrid);
    }
    setToggleMap((prevToggle) => !prevToggle);
    setScore((prevScore) => prevScore + 10);
  };
  return {
    grid,
    moveUp,
    moveDown,
    moveLeft,
    moveRight,
    startGame,
    recordTimerHandler,
    stopGame,
    setCurrentMazeName,
    setStopGame,
    trueScore,
  };
};
```

# My Stats

[![Anurag's GitHub stats](https://github-readme-stats.vercel.app/api?username=immanuelcurrah&theme=moltack&show_icons=true)](https://github.com/anuraghazra/github-readme-stats)
[![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=immanuelcurrah&theme=moltack&show_icons=true)](https://github.com/immanuelcurrah/github-readme-stats)
