<template>
  <container id = "appcontainer">
    <div id = "title">
      <h1>Lost Cities Score Calculator</h1>
    </div>
    <div id = "options">

    </div>
    <div id = "players">
      <div id = "player1">
        <h1>Player 1</h1>
        <AllCards :player-idx = 0 @playerScore = "calParentPlayerScore"></AllCards>
      </div>
      <div id = "player2">
        <h1>Player 2</h1>
        <AllCards :player-idx = 1 @playerScore = "calParentPlayerScore"></AllCards>
      </div>
    </div>
    <div id="result">

    </div>
    <div id="totalresult">

    </div>
  </container>
</template>

<script>
import AllCards from "@/components/AllCards";
export default {
  name: 'App',
  components: {
    AllCards
  },
  data(){
    return {
      roundIdx: 0,
      players: [
        {name: "player1", score: [0, 0, 0, 0, 0], },
        {name: "player2", score: [0, 0, 0, 0, 0], },
      ]
    }
  },
  methods: {
    calParentPlayerScore(player){ //player emitted from the child component
      console.log("player score updated: ", player)
      this.players[player.playerIdx].score[this.roundIdx] = player.playerScore;
    }
  }
}
</script>

<style>
@import './style.css';

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
</style>
