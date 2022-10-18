<template>
<div>
  <div class="tictactoe-board" v-if="start">
    <div v-for="(n, i) in 3" :key="i">
      <div v-for="(n, j) in 3" :key="j">
        <CellComponent @click="performMove(i, j)" :value="board.cells[i][j]"></CellComponent>
      </div>
    </div>
   
   <!--  <div class="game-over-text" v-if="gameOver">
      
       
    </div>   -->
    <div v-if="haveWinner" class="win" >
        <h1>{{gameOverText}}</h1>
        <button @click="newGame" class="btn2">Play Again?</button>
    </div>
  </div>
   <div v-if="!start" class="sart"> 
        <h1>Please choose your symbol</h1>
        <button @click="startGame(2)" class="btn">X</button>
        <button @click="startGame(1)" class="btn">O</button>
    </div>
</div>
</template>
<script>
import CellComponent from './Cell.vue'
import Board from './Board.js'

export default {
    name:'App',
    components:{
      CellComponent,
      
      
    },
    data() { return {
      gameOver: false,
      gameOverText: '',
      board: new Board(),
      start:false,
      player1:'',
      computer:'',
     // msg2:false,
     haveWinner:false,
    } },
     methods: {
      newGame(){
        this.start=false
        this.gameOver=false
        this.board= new Board()
        this.gameOverText= ''
        this.player1=''
        this.computer=''
        this.haveWinner=false
      },
      startGame(k){
          if(k===2)
          {
            this.player1='x'
            this.computer='o'
          }
          else{

            this.player1='o'
            this.computer='x'
          }
          this.start=true
      },
      performMove(x, y) {
        if (this.gameOver) {
          return;
        }
        if (!this.board.doMove(x, y, this.player1)) {//'x' was passed here instead of player
          // Invalid move.
          return;
        }
        //this.$forceUpdate();
        if (this.board.isGameOver()) {
          this.gameOver = true;
          //this.start=false;

          this.gameOverText = this.board.playerHas3InARow(this.player1) ? 'You win!' : 'Draw';//'x' was passed here instead of player
          this.haveWinner=true;

          return;
        }
        let aiMove = this.minimax(this.board.clone(), this.computer);//'o' passed in computer
        this.board.doMove(aiMove.move.x, aiMove.move.y,this.computer);//'o' passed in computer
        if (this.board.isGameOver()) {
          this.gameOver = true;
          //this.start=false;
          this.gameOverText = this.board.playerHas3InARow(this.computer) ? 'You lose!' : 'Draw';//'o' passed in computer
          //this.message(this.gameOverText)
          this.haveWinner=true;

        }
       // this.$forceUpdate();
      },
       minimax(board, player, depth = 1) {
    // If the board has 3 in a row return the score of the board.
        if (board.isGameOver()) {
          return {
            score: board.getScore(this.player1,this.computer) + depth,//() first for score
            move: null
            
          }
        }

    // The 'o' player wants to maximize its score, the 'x' player wants to
    // minimize its score
    let bestScore = player === this.computer ? -10000 : 10000;//'o' for computer
    let bestMove = null;

          let moves = board.getPossibleMoves();
          for (let i = 0; i < moves.length; i++) {
            let move = moves[i];
            let newBoard = board.clone();
            newBoard.doMove(move.x, move.y, player);

            // Recursively call the minimax function for the new board.
            const score = this.minimax(newBoard, player === this.player1 ? this.computer : this.player1, depth + 1).score;
            //==='x' ? 'o' : 'x', depth + 1).score; instead of above

            // If the score is better than the best saved score update the best saved
            // score to this move.
            if ((player === this.computer && score > bestScore) || (player === this.player1 && score < bestScore)) {
              //player==='o'   || player === 'x'
              bestScore = score;
              bestMove = move;
            }
          }

          // Return the best found score & move!
          return {
            score: bestScore,
            move: bestMove
          }
        }
    }
  }
</script>

<style>
  body{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .tictactoe-board {
    display: flex;
    flex-wrap: wrap;
    width: 500px;
    height: auto;
  }
  .btn{
    padding: 10px;
    font-size: 2rem;
    margin: 10px;
    border: 1px solid #333;
    color:blanchedalmond;
    width: 100px;
    background: crimson;
  }
  .btn2{
    padding: 10px;
    font-size: 1rem;
    font-weight: bold;
    margin: 8px;
    border: 1px solid #333;
    color:blanchedalmond;
    width: 90px;
    background: crimson;
  }
  .win{
    background: rgba(175, 135, 50, 0.856);
    color:black;
    width:300px;
    height: 200px;
    text-align: center;
    position: absolute;
    padding: 10px;
    margin:20px;
    border-radius: 4px;
  }
  h1{
    color:rgb(7, 7, 7);
    border-radius: 4px;
    padding: 20px;
      box-shadow: 2px 2px 34px  rgba(67, 67, 70, 0.5);
  }
  .sart{
    text-align: center;
    padding-top:100px;
    
  }
</style>