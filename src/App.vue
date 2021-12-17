<template>
  <div class='main'>
    <div v-for="(element, index) in labelQuiz.slice(a, b)" :key="index" class="mainLabel">
      <h2>{{element.label}}</h2>
    </div>

    <div class='fist' v-if = 'a === 0'>
      <label v-for="houseType in typeOfHouse" :key="houseType.id" class="option">
          <input type="checkbox" v-model="selected" v-bind:value="houseType" class='radioButton'>
          <span class="check__box"></span>
          {{houseType}}
      </label>
    </div>

    <div class='sec' v-if = 'a === 1'>
          <input type="range" min="0" max="100000000" step="1" v-model="countRange"> 
          <input type="text" v-model="countRangeValue"/>
    </div>

    <div class='pre-las-block'>
      <input type="text"
      v-model="phone"
      id="phone"
      v-imask="phoneNumberMask"
      placeholder="+7"
      @keypress="isNumber"
      @accept="onAccept"
      @complete="onComplete"
      maxlength="16"
      v-if = 'a === 2'/>
      
      <button v-if="a === 2" v-on:click="next">Отправить</button>
    </div>

    <div class='las-block' v-if="a === 3">
      <h2>Спасибо!</h2>
      <h3>Наш менеджер скоро свяжется с вами</h3>
      <img :src="require('./img/keys.svg')"/>
    </div>

    <div class='buttonBlock' v-if = 'a < 2'>
    <button v-on:click="prev" class="backWard"><img :src="require('./img/leftRow.svg')" class="leftRow"/></button>
    <button v-on:click="next" class="nextButton"><div class="reiwqizx"><div class="onWard">Далее</div><img :src="require('./img/rightRow.svg')"/></div></button>
    </div>
  </div> 
</template>

<script>
import { IMaskDirective } from 'vue-imask';

export default {
  name: 'App',
  data () {
    return{
      phoneNumberMask: {
        mask: '+{7}(000)000-00-00',
        lazy: true
      },
      typeOfHouse: ['Студия','1-комнатная','2-комнатная','3-комнатная'],
      labelQuiz: [
        {
          label: 'Какой тип квартиры вас интересует?',
        },
        {
          label: 'Стоимость квартиры',
        },
        {
          label: 'Найдены квартиры по вашему запросу',
        },
      ],
      a:0,
      b:1,
      selected: [], 
      countRange: 1000, 
    }
  },
  methods: {
    next(){this.a++; this.b++;},
    prev(){
      if(this.a > 0){
        this.a--; this.b--;
      }else{
        return
      }
    },
    onAccept (e) {
      const maskRef = e.detail;
      this.value = maskRef.value;
      console.log('accept', maskRef.value);
    },
    onComplete (e) {
      const maskRef = e.detail;
      console.log('complete', maskRef.unmaskedValue);
    },
    isNumber(e){
      let regex = /[0-9]/
      if(!regex.test(e.key)){
        e.retrunValue = false;
        if(e.preventDefault) e.preventDefault();
      }
    }
  },
  computed: {
      countRangeValue:{
        get() {
            const IntlNumber = new Intl.NumberFormat(); 
            return IntlNumber.format(this.countRange)
        },
        set (val) {
          if (Number.isNaN(val)) {
          return;
          }
        let newVal = val.replaceAll(/\s/g, "");
        
        if (newVal === 0 || newVal === "") {
            this.countRange = 0;
        } else {
            this.countRange = newVal.replaceAll(/\s/g, "");
        }
        },
      }
  },
  directives: {
    imask: IMaskDirective
  }
}


</script>

<style lang="scss">
.option{
  display: block;
  margin-bottom: 14px;
  margin-left: 100px;
}
.main{
    width: 320px;
    height: 607.35px;
    background: #F7AB0D;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-right: -50%;
    transform: translate(-50%, -50%);
    align-items: center;
    justify-content: center;
}
.mainLabel{
  color: white;
}
.nextButton{
  width: 196px;
  background: #E40505;
  color: white;
  margin-left: 10px;
}
.reiwqizx{
  display: flex;
  align-items: center;
  margin-left: auto;
  margin-right: auto
}.buttonBlock{justify-content: center;}
.nextButton,
.buttonBlock,
.backWard{
  display: flex;
  align-items: center;
  height: 65px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
}
.backWard{
  width: 65px;
}
.onWard{
  margin-right: 64px;
  font-size: 28px;
}
.leftRow{
  margin-left: auto;
  margin-right: auto
}
.radioButton{
  position: absolute;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}
.check__box{
  position: absolute;
  width: 30px;
  height: 30px;
  border-radius: 10px;
  background-color: white;
  margin-left: -40px;
  margin-bottom: 0.25rem;
}
.radioButton:checked + .check__box{
  background: url(./img/checked.svg) no-repeat;
}
</style>
