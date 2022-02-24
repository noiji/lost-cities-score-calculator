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
        <AllCards :player-idx = 0 :is-expand = "isExpand"
                  @playerScore = "calParentPlayerScore" ref="allCards1"></AllCards>
      </div>
      <div id = "player2">
        <h1>Player 2</h1>
        <AllCards :player-idx = 1 :is-expand = "isExpand"
                  @playerScore = "calParentPlayerScore" ref="allCards2"></AllCards>
      </div>
    </div>
    <div id="result">
      <button id="showResultButton" v-if="!roundResultShown" @click="showRoundResult">Show the Result</button>
      <div id="resultDetail" v-if="roundResultShown">
        <div id="resultTitle">ROUND {{roundIdx + 1}}: {{roundWinner}}</div>
        <p>
        <span class="roundScore">{{players[0].score[roundIdx]}} : {{players[1].score[roundIdx]}}</span>
        </p>
        <button id="nextButton" v-if="roundIdx < 4" @click="nextRound">NEXT ROUND</button>
      </div>
    </div>
    <div id="totalresult">
      <Scoreboard :players = players :key="'scoreBoard' + roundWinner"></Scoreboard>
    </div>
  </body>
</template>

<script>
import AllCards from "@/components/AllCards";
import Scoreboard from "@/components/Scoreboard";
import JSConfetti from 'js-confetti'

const confetti = new JSConfetti()

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
      // console.log("player score updated: ", player)
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
    calSum(){
      for (let idx = 0; idx < 2; idx++){
        let sum = 0
        for (let i = 0; i < 5; i++){
          sum += this.players[idx].score[i]
        }
        this.players[idx].sum = sum;
      }
    },
    toggleExpand(){
      this.isExpand = !this.isExpand
      console.log("expand: ", this.isExpand)
    },
    showRoundResult(){
      this.roundResultShown = !this.roundResultShown
      this.roundWinner = this.calRoundWinner()
      this.showConfetti()
      this.calSum()//total score
    },
    resetAppCards(){
      console.log("Player 1 resetCard")
      this.$refs.allCards1.resetAllCards()
      console.log("Player 2 resetCard")
      this.$refs.allCards2.resetAllCards()
    },
    initRound(){
      this.roundResultShown = false
      //TODO: 클릭된 카드 클릭 해제
      this.resetAppCards()
    },
    nextRound(){
      this.roundIdx++
      this.initRound()
    },
    showConfetti() {
      confetti.addConfetti({emojis: ['🌈', '🦄️', '✨', '💫',],})
    },
  },
  computed:{

  },
}
</script>

<style >
@import './style.css';
template{
  background-color: black;
}
body{
  /*width: 520px;*/
  background-color: black;
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
  font-family: Bangers ;
  font-size: 18px;
}
#gamemode{
  font-family: Bangers ;
  font-weight: bold;
  font-size: 25px;
  padding-bottom: 10px;
}
#gamemode a{
  padding-right: 8px;
}
#roundInfo{
  font-family: Bangers;
  font-size: 30px;
}
.exbutton{
  background-color: white;
  border-radius: 5%;
  font-family: "Bangers" ;
  /*font-weight: bold;*/
  font-size: 20px;
  color: grey;
}
.active{
  background-color: limegreen;
  color: black;
}
#result{
  min-height: 80px;/*display: block;*/
  padding: 10px 0;
  font-family: Bangers;
}
.roundScore{
  font-size: 100px;
  padding: 0 10px;
  color: dodgerblue;
  font-weight: bold;
}
#player1, #player2{
  font-size: 12px;
}
#resultDetail p{
  display: flex;
  justify-content: center;
  align-items: center;
}
#showResultButton, #nextButton{
  width: 160px;
  background-color: dodgerblue;
  height: 30px;
  font-family: Bangers;
  font-size: 20px;
  color: white;
  border-radius: 5%;
  margin-left: 100px;
}
Scoreboard{
  padding-left: 20px;
}
#resultTitle{
  font-size: 40px;
  text-align: center;
}
</style>
