<!-- 색깔 당 카드 (invest 1, 2, 3, num 2, 3, ... 10) -->
<template>
  <div id = "section" :style="cssProps">

    <div id = "cardtitle">
     {{ singleColor.name }} : {{singleColor.score}}
    </div>
    <div id = "cards">
      <ul>
        <!--각 element(혹은 component)에 key 값을 바인딩해주면 된다.
        Vue에서는 HTML 요소의 key 값이 변경되는 경우 해당 요소를 업데이트한다. -->
        <button v-if="investcard[0].display" @click="updateInvestCard(0)"
                :key = "investcard[0].id + investcard[0].name + investcard[0].on" v-bind:class="{clickedButton: investcard[0].on}">
            {{ investcard[0].name }}
        </button>
        <button v-if="investcard[1].display" @click="updateInvestCard(1)"
                :key = "investcard[1].id + investcard[1].name + investcard[1].on" v-bind:class="{clickedButton: investcard[1].on}">
            {{ investcard[1].name }}
        </button>
        <button v-if="investcard[2].display" @click="updateInvestCard(2)"
                :key = "investcard[2].id + investcard[2].name + investcard[2].on" v-bind:class="{clickedButton: investcard[2].on}">
            {{ investcard[2].name }}
        </button>
        <button v-for="item in numcard" @click="updateNumCard(item.value)"
                :key="item.name + item.on" v-bind:class="{clickedButton: item.on}">
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
    singleColor: Object, //{id: 0, name: "Yellow", score: 0, isInit: false},
    // isInit: Boolean,
  },
  data() {
    return {
      investnums: 0,
      sumscore: -20,
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
      totalcardcnt: 0, //필요 1. default 점수 0 또는 -20 중 선택 2. 8개 카드 이상 획득 시 보너스 점수
    }
  },
  methods: {
    resetCards(){
      console.log(this.singleColor.name, ': reset')
      for (let i = 0; i < 3; i++) {
        this.investcard[i].on = false;
        this.investcard[i].display = (i === 0);
      }
      for (let i = 2; i <= 10; i++) {
        this.numcard[i-2].on = false;
      }
      this.investnums = 0
      this.totalcardcnt = 0
      this.sumscore = -20
      this.singleColor.score = 0
    },
    calScore() {
      if (this.totalcardcnt > 0){
        let score = this.sumscore * (this.investnums + 1)
        let bonus = ( this.totalcardcnt >= 8 )? 20: 0
        this.singleColor.score = score + bonus
        // this.$emit('updateSingle', this.singleColor ) // prop으로 들어온 object를 바꾸면 emit을 안해도 되게 됨.
      }
      else {
        this.singleColor.score = 0
      }
    },
    updateSumScore(x) {
      this.sumscore += x;
      this.calScore();
    },
    updateInvestCard(x) { //앞 또는 뒤 카드의 display를 변경해줘야 한다.
      console.log("update Invest Card")
      if (!this.investcard[x].on) { //꺼진 카드를 켜준다. 즉 invest card 개수가 는다.
        this.investnums++;
        this.totalcardcnt++;
      } else { //켜진 카드를 꺼준다.
        this.investnums--;
        this.totalcardcnt--;
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
      this.calScore()
    },
    updateNumCard(x){
      console.log("update Num Card")

      if (!this.numcard[x-2].on){
        this.numcard[x-2].on = true;
        this.totalcardcnt++;
        this.updateSumScore(x)
      }
      else {
        this.numcard[x-2].on = false;
        this.totalcardcnt--;
        this.updateSumScore(x * (-1))
      }
    }
  },
  computed: {
    cssProps() {
      return {
        '--card-color': this.singleColor.name,
        '--title-color': this.singleColor.name,
      }
    },
  },
}
</script>

<style scoped>
#section{
  color: var(--title-color);
  margin-bottom: 30px;
}
#cards{
  padding-bottom: 4px;
}
#cards ul{
  list-style-type: none;
  margin: 0; /* these two make the list align left */
  padding: 0;
}
#cards button{
  float: left; /*  리스트를 가로로 정렬*/
  border-color: var(--card-color);
  padding: 8px;
  font-family: "Special Elite";
  border-radius: 20%;
  margin: 1px;
  font-size: 15px;
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
#cardtitle{
  font-size: 20px;
  padding-bottom: 5px;
  font-family: Bangers;
  /*font-weight: bold;*/
}
.clickedButton{
  background-color: var(--card-color);
  color: black;
  font-weight: bold;
}
</style>