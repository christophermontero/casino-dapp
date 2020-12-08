<template>
  <data-metamask />
</template>
<script>
import DataMetamask from "@/components/data-metamask";
export default {
  name: "casino-dapp",
  beforeCreate() {
    console.log("registerWeb3 Action dispatched from casino-dapp.vue");
    this.$store.dispatch("registerWeb3");
  },
  components: {
    "data-metamask": DataMetamask,
  },
};
</script>
<style scoped></style>
