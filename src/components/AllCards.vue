<template>
  <div>
    <div v-if="isExpand">
      <Cards v-for="cardStack in allColor" :key = "cardStack" :single-color="cardStack"
             @change="updatePlayerScore"></Cards>
    </div>
    <div v-else>
      <Cards v-for="cardStack in allColor.slice(0, 5)" :key = "cardStack" :single-color="cardStack"
             @change="updatePlayerScore"></Cards>
    </div>
  </div>
</template>

<script>
import Cards from "./Cards.vue"
//TODO
//색깔별 점수 총합 후 App에 넘기기
export default {
  name: "AllCards",
  components: {
    Cards,
  },
  props:{
    isExpand: Boolean,
    playerIdx: Number,
  },
  data(){
    return {
      msg: "test",
      allColor: [
        {id: 0, name: "Yellow", score: 0},
        {id: 1, name: "White", score: 0},
        {id: 2, name: "Aqua", score: 0},
        {id: 3, name: "Green", score: 0},
        {id: 4, name: "Red", score: 0},
        {id: 5, name: "Purple", score: 0},
      ],
      playerScore: this.updatePlayerScore,
    }
  },
  methods:{
  },
  computed: {
    updatePlayerScore() {
      let sum = 0
      let len = this.allColor.length
      for (let i = 0; i < len; i++){
        sum += this.allColor[i].score
      }
      this.$emit('playerScore', {playerIdx: this.playerIdx, playerScore: sum})
      return sum
    }
  },
}
</script>

<style scoped>

</style>