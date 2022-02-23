<template>
  <body id = "appcontainer">
    <div id = "title">
      <h1>Lost Cities Score Calculator</h1>
    </div>
    <div id = "gamemode">
      <a>GAME MODE </a>
      <button class = "exbutton" :class="{active: !isExpand}" @click="toggleExpand">
        NORMAL</button>
      <button class = "exbutton" :class="{active: isExpand}" @click="toggleExpand">
        EXPAND</button>
    </div>
    <div id="roundInfo">
      <a> ROUND {{roundIdx + 1}}</a>
    </div>
    <div id = "players">
      <div id = "player1">
        <h1>Player 1</h1>
        <AllCards :player-idx = 0 :is-expand = "isExpand" @playerScore = "calParentPlayerScore"></AllCards>
      </div>
      <div id = "player2">
        <h1>Player 2</h1>
        <AllCards :player-idx = 1 :is-expand = "isExpand" @playerScore = "calParentPlayerScore"></AllCards>
      </div>
    </div>
    <div id="result">
      <button id="showResultButton" v-if="!roundResultShown" @click="showRoundResult">Show Result</button>
      <div id="resultDetail" v-if="roundResultShown">
        <p>ROUND {{roundIdx + 1}}: {{roundWinner}}</p>
        <span class="roundScore">{{players[0].score[roundIdx]}}</span>
        <span class="versus">vs</span>
        <span class="roundScore">{{players[1].score[roundIdx]}}</span>
        <br>
        <button v-if="roundIdx < 4" @click="nextRound">NEXT ROUND</button>
      </div>
    </div>
    <div id="totalresult">
      <Scoreboard :players = players></Scoreboard>
    </div>
  </body>
</template>

<script>
import AllCards from "@/components/AllCards";
import Scoreboard from "@/components/Scoreboard";

export default {
  name: 'App',
  components: {
    Scoreboard,
    AllCards,
  },
  data(){
    return {
      roundIdx: 0,
      players: [
        {name: "player1", score: [0, 0, 0, 0, 0], sum: 0},
        {name: "player2", score: [0, 0, 0, 0, 0], sum: 0},
      ],
      isExpand: false,
      roundResultShown: false, // needs initialization after a round
      roundWinner: "",
    }
  },
  methods: {
    calParentPlayerScore(player){ //player emitted from the child component
      console.log("player score updated: ", player)
      this.players[player.playerIdx].score[this.roundIdx] = player.playerScore;
    },
    calRoundWinner(){
      if (this.players[0].score[this.roundIdx] > this.players[1].score[this.roundIdx])
        return "Player 1 Wins!";
      else if (this.players[0].score[this.roundIdx] == this.players[1].score[this.roundIdx]){
        return "It's a Tie!"
      }
      else{
        return "Player 2 Wins!"
      }
    },
    toggleExpand(){
      this.isExpand = !this.isExpand
      console.log("expand: ", this.isExpand)
    },
    showRoundResult(){
      this.roundResultShown = !this.roundResultShown
      this.roundWinner = this.calRoundWinner()
    },
    initRound(){
      this.roundResultShown = false
      //클릭된 카드 클릭 해제

    },
    nextRound(){
      this.roundIdx++
      this.initRound()
    }
  },
  computed:{

  }
}
</script>

<style >
@import './style.css';
body{
  float: left;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#title{
  font-family: Permanent Marker ;
}
#gamemode{
  font-family: "Source Sans Pro" ;
  font-weight: bold;
  font-style: italic;
  font-size: 20px;
  padding-bottom: 10px;
}
#options a{
  padding-right: 8px;
}
#roundInfo{
  font-family: "Permanent Marker";
  font-weight: bold;
  font-size: 30px;
}
.exbutton{
  background-color: antiquewhite;
  padding: 3px;
  border-radius: 5%;
  font-family: "Source Sans Pro" ;
  font-weight: bold;
  font-size: 15px;
}
.active{
  background-color: limegreen;
}

#result{
  min-height: 80px;/*display: block;*/
}
.roundScore{
  font-size: 80px;
  padding: 0 10px;
  color: dodgerblue;
  font-weight: bold;
}
#player1, #player2{
  font-size: 12px;
  padding: 0 0 15px 0;
}
</style>
