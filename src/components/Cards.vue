<template>
  <div id = "section" :style="cssProps">

    <div id = "cardtitle">
<!--      -->
     <h3>{{ color }}</h3>
<!--      <p> test</p>-->
    </div>
    <div id = "cards">
      <ul>
        <li v-if="investcard[0].display" >
          <a href="">
            {{ investcard[0].name }}
          </a>
        </li>
        <li v-if="investcard[1].display">
          <a href="">
            {{ investcard[1].name }}
          </a>
        </li>
        <li v-if="investcard[2].display">
          <a href="">
            {{ investcard[2].name }}
          </a>
        </li>
        <li v-for="item in numcard" @click="item.updateNumCard" v-bind:key="item">
          <a href="">
          {{ item.name }}
          </a>
        </li>
        <br>
      </ul>
    </div>
    <br>
  </div>

</template>

<script>
export default {
  name: "Cards",
  props: {
    color: String,
  },
  data() {
    return {
      investnums: 0,
      sumscore: -20,
      returnscore: 0,
      investcard: [
        {id: 0, name: '§', on: false, display: true},
        {id: 1, name: '§', on: false, display: false},
        {id: 2, name: '§', on: false, display: false},
      ],
      numcard: [
            {value: 2, name: '2', on: false,},
            {value: 3, name: '3', on: false,},
            {value: 4, name: '4', on: false,},
            {value: 5, name: '5', on: false,},
            {value: 6, name: '6', on: false,},
            {value: 7, name: '7', on: false,},
            {value: 8, name: '8', on: false,},
            {value: 9, name: '9', on: false,},
            {value: 10, name: '10', on: false,},
          ],
    }
  },
  methods: {
    changeInvestments(){
      this.investnums++;
    },
    toggleCard(){
    },
    calScore(){

    },
    updateSumScore(x){
      this.sumscore += x
    },
    updateInvestCard(){ //앞 또는 뒤 카드의 display를 변경해줘야 한다.
      if (!this.on){
        this.on = true;
        this.updateSumScore(this.value)
      }
      else {
        this.on = false;
        this.updateSumScore(this.value * (-1))
      }
    },
    updateNumCard(){
      if (!this.on){
        this.on = true;
        this.updateSumScore(this.value)
      }
      else {
        this.on = false;
        this.updateSumScore(this.value * (-1))
      }
    }
  },
  computed: {
    cssProps(){
      return {
        '--card-color': this.color,
      }
    }
  }
}
</script>

<style scoped>
#section{
  color: var(--card-color);
}
#cards ul{
  list-style-type: none;
  margin: 0; /* these two make the list align left */
  padding: 0;
}
#cards li{
  float: left; /*  리스트를 가로로 정렬*/
  /*height: 30px;*/
  background-color: var(--card-color);
  border-color: var(--card-color);
  padding: 10px;
}
#cards li a{
  text-decoration: none; /* remove link underline */
  padding: inherit;
  color: black;
}
#cards li a:hover {
  background-color: white;
}
</style>