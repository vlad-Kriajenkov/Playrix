<template>
    <div class="home">
        <div class="home__header">
            <div class="header__nameLogo">
                <img src="../assets/img/logotype.svg" alt="">
            </div>
            <div class="header__nameImg">
                <img src="../assets/img/unnamed.png" alt="">
            </div>
        </div>
        <div class="home__main">
            <div class="main__conteiner1">
              <div class="main__title">
                Конвертер валют
              </div>
              <form class="main__converterContainer">
                  <div class="converterContainer__converterChanging">
                      <label class="converter__lable" for="">
                        У меня есть
                        <div class="converter__input">
                          <input  type="text" name="" id=""  v-model="serch">  
                        </div>
                        <v-select 
                          class="converter__select"
                          :options="books" 
                          label="title"
                          v-model="selected"
                        ></v-select>
                      </label>    
                      <div class="converter__infoOneValue"> 1 {{selected.title}} = {{valute[selected2.title]}} {{selected2.title}}</div>
                  </div>
                  <div 
                  @click="reverse"
                  class="converterContainer__Btn">
                    <img src="../assets/img/swap.svg" alt="">
                  </div>
                  <div class="converterContainer__converterReceiving">
                    <label class="converter__lable" for="">
                        Я получу
                        <div class="converter__infoChanging"  > {{sum}}</div>
                        <v-select 
                          class="converter__select"
                          :options="books" 
                          label="title"
                          v-model="selected2"
                        ></v-select>
                    </label>    
                    <div class="converter__infoOneValue"> 1 {{selected2.title}} = {{sum1}} {{selected.title}}</div>
                  </div>
              </form>
            </div>
            <div class="main__conteiner2">
              <div class="conteiner2__title">
                Список стоимостей
              </div>
              <div class="conteiner2__listValues">
                <div class="listValues__colum">
                  <div class="colum__name">
                    <div class="name__one">
                      {{selected.title}}
                    </div> 
                    <div class="name__two"> 
                      {{selected2.title}}
                    </div>
                  </div>
                 <div class="wrapperColum">
                    <div>
                      <div 
                        class="colum__listStaticNum"
                        v-for="listCost, index in listCosts"
                        :key="index">
                        {{listCost.stakeMoney}} {{selected.title}}
                        <img src="../assets/img/chevron-right.png" alt="">
                        <div class="colum__listDinamicNum">
                          {{listCost.allmani}} {{selected2.title}}
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                 <div class="listValues__colum">
                  <div class="colum__name">
                    <div class="name__one">
                      {{selected2.title}}
                    </div> 
                    <div class="name__two"> 
                      {{selected.title}}
                    </div>
                  </div>
                 <div class="wrapperColum">
                    <div>
                      <div 
                        class="colum__listStaticNum"
                        v-for="listCost, index in listCosts1"
                        :key="index">
                        {{listCost.stakeMoney}} {{selected2.title}}
                        <img src="../assets/img/chevron-right.png" alt="">
                        <div class="colum__listDinamicNum">
                          {{listCost.allmani}} {{selected.title}}
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

              </div>
            </div>
        </div>
        <div>

        </div>
    </div>
</template>

<script>
export default {
     components: {
     
    },
    data(){
        return{
          valute: [],
          books: [
            { title: "EUR" },
            { title: "UAH" },
            { title: "RUB" },
            { title: 'USD' }
          ],
          selected:'',
          selected2:'',
          serch:'',
          result: '',
          valute: '',
          valute1:'',
          sum: '',
          sum1: '',
          listCosts: [
            {stakeMoney: '1', allmani:'' },
            {stakeMoney: '5', allmani:''},
            {stakeMoney: '10', allmani:''},
            {stakeMoney: '25', allmani:''},
            {stakeMoney: '50', allmani:''},
            {stakeMoney: '100', allmani:''},
            {stakeMoney: '500', allmani:''},
            {stakeMoney: '1000', allmani:''},
            {stakeMoney: '5000', allmani:''},
          ],
          listCosts1: [
            {stakeMoney: '1', allmani:'' },
            {stakeMoney: '5', allmani:''},
            {stakeMoney: '10', allmani:''},
            {stakeMoney: '25', allmani:''},
            {stakeMoney: '50', allmani:''},
            {stakeMoney: '100', allmani:''},
            {stakeMoney: '500', allmani:''},
            {stakeMoney: '1000', allmani:''},
            {stakeMoney: '5000', allmani:''},
          ]
        }
    },
     mounted(){
      fetch('https://api.fastforex.io/fetch-multi?from=USD&to=EUR,UAH,RUB,USD&api_key=12d16f39b0-d44edd54de-r64egt')
        .then(response => response.json())
        .then(json =>  {
            this.valute = json.results
            // console.log(this.valute)
          })
        .catch(err => console.error(err));
    },
    methods:{
      reverse(){
        let temp
        temp = this.selected
        this.selected = this.selected2;
        this.selected2 = temp;
      }
    },
    watch:{
      serch(){
        const selec = 'USD';
        let summ = this.serch * this.valute[this.selected2.title] / this.valute[this.selected.title]
        this.sum = summ;

        if (this.selected.title == selec) {
          let summ1 = 1 / this.valute[this.selected2.title]
          this.sum1 = summ1;
          console.log('sd')
        } else{
          let summ1 = 1 * this.valute[this.selected2.title] / this.valute[this.selected.title]
          this.sum1 = summ1;
        }
   
        this.listCosts[0].allmani = this.sum1 * 1
        this.listCosts[1].allmani = this.sum1 * 5
        this.listCosts[2].allmani = this.sum1 * 10
        this.listCosts[3].allmani = this.sum1 * 25
        this.listCosts[4].allmani = this.sum1 * 50
        this.listCosts[5].allmani = this.sum1 * 100
        this.listCosts[6].allmani = this.sum1 * 500
        this.listCosts[7].allmani = this.sum1 * 1000
        this.listCosts[8].allmani = this.sum1 * 5000

        this.listCosts1[0].allmani = this.valute[this.selected2.title] * 1
        this.listCosts1[1].allmani = this.valute[this.selected2.title] * 5
        this.listCosts1[2].allmani = this.valute[this.selected2.title] * 10
        this.listCosts1[3].allmani = this.valute[this.selected2.title] * 25
        this.listCosts1[4].allmani = this.valute[this.selected2.title] * 50
        this.listCosts1[5].allmani = this.valute[this.selected2.title] * 100
        this.listCosts1[6].allmani = this.valute[this.selected2.title] * 500
        this.listCosts1[7].allmani = this.valute[this.selected2.title] * 1000
        this.listCosts1[8].allmani = this.valute[this.selected2.title] * 5000

        console.log(this.listCosts[0].stakeMoney )
        console.log(this.serch)
        console.log(this.valute.RUB)
      },
    }
    
}
</script>
