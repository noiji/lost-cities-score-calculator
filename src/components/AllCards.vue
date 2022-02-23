<template>
  <div>
    <Cards :color="colors[0]"></Cards>
    <Cards :color="colors[1]"></Cards>
    <Cards :color="colors[2]"></Cards>
    <Cards :color="colors[3]"></Cards>
    <Cards :color="colors[4]"></Cards>
    <Cards :color="colors[5]" :v-if="isExpand"></Cards>
  </div>
</template>

<script>
import Cards from "./Cards.vue"
export default {
  name: "AllCards",
  components: {
    Cards,
  },
  props:{
    isExpand: Boolean
  },
  data(){
    return {
      msg: "test",
      colors: ["Yellow", "White", "Blue", "Green", "Red", "Purple"],
    }
  },
  methods: {

  }
}
</script>

<style scoped>

</style>