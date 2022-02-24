<template>
  <table>
    <tr>
      <th></th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>Total</th>
    </tr>
    <tr>
      <td>Player 1</td>
      <td v-for="i in 5" :key="'player 1:' + i">{{players[0].score[i-1]}}</td>
      <td>{{players[0].sum}}</td>
    </tr>
    <tr>
      <td>Player 2</td>
      <td v-for="i in 5" :key="'player 2:' + i">{{players[1].score[i-1]}}</td>
      <td>{{players[1].sum}}</td>
    </tr>
  </table>
</template>

<script>
export default {
  name: "Scoreboard",
  props: {
    players: Array,
    // [
    //   {name: "player1", score: [0, 0, 0, 0, 0], },
    //   {name: "player2", score: [0, 0, 0, 0, 0], },
    // ],
  }
}
</script>

<style scoped>
table{
  width: 90%;
  font-size: 15px;
  border: 5px solid #444444;
  border-collapse: collapse;


}
th, td{
  border: 2px solid white;
  padding: 5px;
}
</style>