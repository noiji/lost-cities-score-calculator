<template>
  <container id = "appcontainer">
    <div id = "title">
      <h1>Lost Cities Score Calculator</h1>
    </div>
    <div id = "options">
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
    AllCards,
  },
  data(){
    return {
      roundIdx: 0,
      players: [
        {name: "player1", score: [0, 0, 0, 0, 0], },
        {name: "player2", score: [0, 0, 0, 0, 0], },
      ],
      isExpand: false,
    }
  },
  methods: {
    calParentPlayerScore(player){ //player emitted from the child component
      console.log("player score updated: ", player)
      this.players[player.playerIdx].score[this.roundIdx] = player.playerScore;
    },
    toggleExpand(){
      this.isExpand = !this.isExpand
      console.log("expand: ", this.isExpand)
    }
  }
}
</script>

<style >
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
#options{
  font-family: "Source Sans Pro" ;
  font-weight: bold;
  font-style: italic;
  font-size: 20px;
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

</style>
