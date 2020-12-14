<template>
  <data-metamask />
</template>
<script>
import DataMetamask from "@/components/data-metamask";
export default {
  name: "casino",
  mounted() {
    console.log("dispatching getContractInstance");
    this.$store.dispatch("getContractInstance");
  },
};
</script>
<style scoped></style>
