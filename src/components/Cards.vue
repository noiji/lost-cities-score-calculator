<!-- 색깔 당 카드 (invest 1, 2, 3, num 2, 3, ... 10) -->
<template>
  <div id = "section" :style="cssProps">

    <div id = "cardtitle">
<!--      -->
     <h3>{{ singleColor.name }}</h3>
<!--      <p> test</p>-->
    </div>
    <div id = "cards">
      <ul>
        <!--각 element(혹은 component)에 key 값을 바인딩해주면 된다.
        Vue에서는 HTML 요소의 key 값이 변경되는 경우 해당 요소를 업데이트한다. -->
        <button v-if="investcard[0].display" @click="updateInvestCard(0)"
                :key = "investcard" v-bind:class="{clickedButton: investcard[0].on}">
            {{ investcard[0].name }}
        </button>
        <button v-if="investcard[1].display" @click="updateInvestCard(1)"
                :key = "investcard" v-bind:class="{clickedButton: investcard[1].on}">
            {{ investcard[1].name }}
        </button>
        <button v-if="investcard[2].display" @click="updateInvestCard(2)"
                :key = "investcard" v-bind:class="{clickedButton: investcard[2].on}">
            {{ investcard[2].name }}
        </button>
        <button v-for="item in numcard" @click="updateNumCard(item.value)"
                :key="item" v-bind:class="{clickedButton: item.on}">
          {{ item.name }}
        </button>
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
    singleColor: Object, //{id: 0, name: "Yellow", score: 0},
  },
  data() {
    return {
      investnums: 0,
      sumscore: -20,
      returnscore: 0, // 이 점수를 emit으로 보내줘야 함
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
    calScore() {
      // this.returnscore = this.sumscore * (this.investnums + 1)
      this.singleColor.score = this.sumscore * (this.investnums + 1)
      // console.log("cal Score ", this.singleColor) //prop으로 들어온 object를 바꾸면 emit을 안해도 되는건가?
      this.$emit('updateSingle', this.singleColor ) // {id: 0, name: "Yellow", score: 0} 자체를 리턴
    },
    updateSumScore(x) {
      this.sumscore += x;
      console.log(this.color, " sumscore: ", this.sumscore);
      this.calScore();
    },
    updateInvestCard(x) { //앞 또는 뒤 카드의 display를 변경해줘야 한다.
      console.log("update Invest Card")
      if (!this.investcard[x].on) { //꺼진 카드를 켜준다. 즉 invest card 개수가 는다.
        this.investnums++;
      } else { //켜진 카드를 꺼준다.
        this.investnums--;
      }

      for (let i = 0; i < 3; i++) { // 투자 개수만큼 카드를 키고 나머지는 끈다.
        if (i < this.investnums) {
          this.investcard[i].on = this.investcard[i].display = true;
        } else {
          this.investcard[i].on = this.investcard[i].display = false;
        }
      }

      if (this.investnums < 3) {
        this.investcard[this.investnums].display = true; //꺼진 다음 카드가 보이게 한다.
      }

      for (let i = 0; i<3; i++){
        console.log(this.color, this.investcard[i].id, this.investcard[i].name, this.investcard[i].on, this.investcard[i].display)
      }

      this.calScore()
    },
    updateNumCard(x){
      console.log("update Num Card")

      if (!this.numcard[x-2].on){
        this.numcard[x-2].on = true;
        this.updateSumScore(x)
      }
      else {
        this.numcard[x-2].on = false;
        this.updateSumScore(x * (-1))
      }
    }
  },
  computed: {
    cssProps(){
      return {
        '--card-color': this.singleColor.name,
        '--title-color': this.singleColor.name,
      }
    }

  }
}
</script>

<style scoped>
#section{
  color: var(--title-color);
}
#cards ul{
  list-style-type: none;
  margin: 0; /* these two make the list align left */
  padding: 0;
}
#cards button{
  float: left; /*  리스트를 가로로 정렬*/
  border-color: var(--card-color);
  padding: 10px;
  font-family: "Special Elite";
  border-radius: 20%;
  margin: 3px;
  font-size: 20px;
}
#cards button:not(.clickedButton){
  background-color: black;
  color: white;
}
#cards li a{
  text-decoration: none; /* remove link underline */
  padding: inherit;
  color: black;
}
#cards li a:hover {
  background-color: white;
}
.clickedButton{
  background-color: var(--card-color);
  color: black;
  font-weight: bold;
}
</style>