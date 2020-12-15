<template>
  <div class="casino container">
    <h1>Bienvenido al Casino-Dapp</h1>
    <h4>Porfavor seleccione un número de 1 a 10</h4>
    Apuesta: <input v-model="amount" placeholder="0 Ether" />
    <ul>
      <li v-on:click="clickNumber"><button class="button button2">1</button></li>
      <li v-on:click="clickNumber"><button class="button button2">2</button></li>
      <li v-on:click="clickNumber"><button class="button button2">3</button></li>
      <li v-on:click="clickNumber"><button class="button button2">4</button></li>
      <li v-on:click="clickNumber"><button class="button button2">5</button></li>
    </ul>
    <img v-if="pending" id="loader" src="../images/blocks.gif" />
    <div class="event" v-if="winEvent">
      <p v-if="winEvent._status" id="has-won">
        <i aria-hidden="true" class="fa fa-check"></i> Felicitaciones, has ganado
        {{ winEvent._amount }} wei
      </p>
      <p v-else id="has-lost">
        <i aria-hidden="true" class="fa fa-times"></i> Lo sentimos has perdido, intenta de
        nuevo.
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "casino",
  data() {
    return {
      amount: null,
      pending: false,
      winEvent: null,
    };
  },
  methods: {
    clickNumber(event) {
      console.log("BETTING ON NUMBER, AMOUNT", event.target.innerHTML, this.amount);
      this.winEvent = null;
      this.pending = true;
      this.$store.state.contractInstance().bet(
        event.target.innerHTML,
        {
          gas: 300000,
          value: this.$store.state.web3.web3Instance().toWei(this.amount, "ether"),
          from: this.$store.state.web3.coinbase,
        },
        (err, result) => {
          if (err) {
            console.log(err);
            this.pending = false;
          } else {
            let Won = this.$store.state.contractInstance().Won();
            Won.watch((err, result) => {
              if (err) {
                console.log("could not get event Won()");
              } else {
                this.winEvent = result.args;
                this.winEvent._amount = parseInt(result.args._amount, 10);
                this.pending = false;
              }
            });
          }
        }
      );
    },
  },
  mounted() {
    console.log("dispatching getContractInstance");
    this.$store.dispatch("getContractInstance");
  },
};
</script>

<style scoped>
.casino {
  margin-top: 50px;
  text-align: center;
}
#loader {
  width: 150px;
}
ul {
  margin: 10px;
  list-style-type: none;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-column-gap: 5px;
  grid-row-gap: 5px;
}
li {
  cursor: pointer;
}
* {
  color: #148f77;
}
#has-won {
  color: green;
}
#has-lost {
  color: red;
}
.button {
  background-color: #4caf50; /* Green */
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}
.button2 {
  background-color: white;
  color: black;
  border: 2px solid #008cba;
}
.button2:hover {
  background-color: #008cba;
  color: white;
}
input {
  border: 2px solid #008cba;
  border-radius: 4px;
}
</style>
