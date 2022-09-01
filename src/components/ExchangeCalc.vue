<template>
  <p class="m-auto pb-3 text-[20px]">Конвертор валют</p>
  <div class="flex justify-center flex-wrap gap-5">
    <div class="flex">
      <input
        @input="getRate"
        v-model.number="quantity"
        type="number"
        placeholder="Введите количество"
        class="focus:outline-none focus:ring focus:border-blue-500 rounded border-2 border-colid w-40 field"
      />
      <select
        @change="getRate"
        v-model="currenTo"
        :value="currenTo"
        class="mx-3 border-blue-500 rounded border-2 border-colid w-[60px]"
      >
        <option v-for="(curren, idx) in currencies" :key="idx">
          {{ curren }}
        </option>
      </select>
    </div>
    <div>
      <img
        src="../assets/img/up-arrow-svgrepo-com.svg"
        alt="Стрелка"
        class="w-10 rotate-90"
      />
    </div>
    <div class="flex">
      <div class="flex border-2 rounded ml-5 w-40 truncate">
        <div class="m-auto w-auto cursor-default select-none">
          {{ valueFinal }}
        </div>
      </div>
      <select
        @change="getRate"
        v-model="currenFrom"
        class="mx-3 border-blue-500 rounded border-2 border-colid w-auto"
      >
        <option v-for="(curren, idx) in currencies" :key="idx">
          {{ curren }}
        </option>
      </select>
    </div>
  </div>
</template>

<script>
  export default {
    beforeMount() {
      this.getList();
    },
    data() {
      return {
        currenTo: null,
        currenFrom: null,
        quantity: null,
        currencies: [],
        rate: null,
      };
    },
    computed: {
      valueFinal() {
        let course = this.rate * this.quantity;
        return course.toFixed(2);
      },
    },
    methods: {
      async getList() {
        var myHeaders = new Headers();
        myHeaders.append("apikey", "fmDCI6kURyel5Gk333vGoR5SY5NzWQ5E");

        var requestOptions = {
          method: "GET",
          redirect: "follow",
          headers: myHeaders,
        };

        const options = {
          method: "GET",
          headers: {
            "X-RapidAPI-Key":
              "289629fe02msh5bed508bc60f5b4p14c29ajsn8c118dbc35a4",
            "X-RapidAPI-Host": "currency-exchange.p.rapidapi.com",
          },
        };

        await fetch(
          "https://currency-exchange.p.rapidapi.com/listquotes",
          options
        )
          .then((response) => response.json())
          .then((result) => {
            this.currencies = result;
          })
          .catch((error) => console.log("error", error));

        const navigatorObj = window.navigator.languages[1];

        await fetch(
          `https://api.apilayer.com/geo/country/code/${navigatorObj}`,
          requestOptions
        )
          .then((response) => response.json())
          .then((result) => {
            this.currenTo = result[0].currencies[0].code;
          })
          .catch((error) => console.log("error", error));
      },
      async getRate() {
        const options = {
          method: "GET",
          headers: {
            "X-RapidAPI-Key":
              "289629fe02msh5bed508bc60f5b4p14c29ajsn8c118dbc35a4",
            "X-RapidAPI-Host": "currency-exchange.p.rapidapi.com",
          },
        };
        await fetch(
          `https://currency-exchange.p.rapidapi.com/exchange?to=${this.currenTo}&from=${this.currenFrom}&q=${this.quantity}`,
          options
        )
          .then((response) => response.json())
          .then((result) => {
            this.rate = result;
          })
          .catch((error) => console.log("error", error));
      },
    },
  };
</script>

<style lang="css">
  input[type="number"]::-webkit-outer-spin-button,
  input[type="number"]::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  input[type="number"] {
    -moz-appearance: textfield;
  }
</style>
