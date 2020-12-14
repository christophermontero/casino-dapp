<template>
  <data-metamask />
</template>
<script>
import DataMetamask from "@/components/data-metamask";
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
<style scoped></style>
