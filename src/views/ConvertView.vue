

<script>
    export default{
      created(){
        let promise = fetch('https://api.freecurrencyapi.com/v1/latest?'+
        'apikey=9rxF6i8YuMa16mWAH3wU7SKFtaAQ3akw8zT1l6Ff&'+
        'currencies=THB%2CNOK%2CTRY%2CHKD%2CBGN%2CEUR%2CCNY%2CCAD%2CSEK%2CBRL%2CCAD%2CUSD')

        promise
            .then(response => {
            //console.log(response)

            let someOtherPromise = response.json()

            return someOtherPromise
            })
            .then(result => {
            this.rates = result.data;
            });
            promise;
        },
        methods: {
            handleSelected(){
                const selectedNumber = this.selected
                const extractNumber = selectedNumber.match(/\d+\.?\d+/g)
                const extractAlpha = selectedNumber.match(/[a-zA-Z]+/g)
                this.currencyCode = extractAlpha?.[0]
                this.selectedRate = extractNumber?.[0]

            },
            updateField2 () {
                this.textfield2 = this.inputField1 *  this.selectedRate
            }
        
        },
        data(){
          return{
            rates: '',
            selected: '',
            inputField1 : '',
            selectedRate: '',
            textfield2 : '',
            currencyCode: ''
          }
        }
    }
</script>

<template>
  <h4>>Konvertera</h4>
<div class="border border-4 convert">
    <h2>Konvertera</h2>
  <div class="select">
    <label>Valuta</label>
      <select 
        v-model = "selected" 
        @click="handleSelected"
        class="form-select">  
        <option 
          v-for="(rate, correncyCode ) in rates">
          {{ correncyCode }} {{ rate.toFixed(2) }}
        </option>
      </select>
  </div>
    <div class="fields">
        <div class="field1">
            <h2>USD</h2>
            <p>Pris: 1 </p>
            <input class="form-control"
                type="text" 
                v-model.number = "inputField1" 
                @keyup = "updateField2" 
                placeholder=" Dollar">
        </div>

        <div class="field2">
            <h2> <span style="font-size: small;">Till</span> {{ currencyCode }} </h2>
            <p>Pris: {{ selectedRate }} </p>
            <input class="form-control"
                type="text" 
                :value="textfield2 + ' ' + currencyCode"
                >
        </div>
    </div>
</div>
</template>