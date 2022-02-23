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
      <td v-for="score in players[0].score" :key="score">{{score}}</td>
    </tr>
    <tr>
      <td>Player 2</td>
      <td v-for="score in players[1].score" :key="score">{{score}}</td>
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

</style>