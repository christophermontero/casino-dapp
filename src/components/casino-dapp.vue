<template>
  <div>
    <casino-component />
    <data-metamask />
  </div>
</template>

<script>
import DataMetamask from "@/components/data-metamask";
import CasinoComponent from "@/components/casino-component";

export default {
  name: "casino-dapp",
  beforeCreate() {
    console.log("registerWeb3 Action dispatched from casino-dapp.vue");
    this.$store.dispatch("registerWeb3");
  },
  components: {
    "data-metamask": DataMetamask,
    "casino-component": CasinoComponent,
  },
};
</script>

<style scoped></style>
