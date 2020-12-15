<template>
  <table id="metamask-info">
    <tr>
      <th>
        <p v-if="isInjected" id="has-metamask">
          <i aria-hidden="true" class="fa fa-check"></i> Metamask installed
        </p>
        <p v-else id="no-metamask">
          <i aria-hidden="true" class="fa fa-times"></i> Metamask not found
        </p>
      </th>
    </tr>
    <tr>
      <td>
        <p>Network: {{ network }}</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>Account: {{ coinbase }}</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>Balance: {{ balance }} Wei // {{ ethBalance }} Eth</p>
      </td>
    </tr>
  </table>
</template>

<script>
import { NETWORKS } from "../util/constants/networks";
import { mapState } from "vuex";
export default {
  name: "hello-metamask",
  computed: mapState({
    isInjected: (state) => state.web3.isInjected,
    network: (state) => NETWORKS[state.web3.networkId],
    coinbase: (state) => state.web3.coinbase,
    balance: (state) => state.web3.balance,
    ethBalance: (state) => {
      if (state.web3.web3Instance !== null)
        return state.web3.web3Instance().fromWei(state.web3.balance, "ether");
    },
  }),
};
</script>

<style scoped>
#has-metamask {
  color: green;
}
#no-metamask {
  color: red;
}
#metamask-info {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}
#metamask-info td,
#metamask-info th {
  border: 1px solid #ddd;
  padding: 2px;
  text-align: center;
}
#metamask-info tr:nth-child(even) {
  background-color: #f2f2f2;
}
#metamask-info tr:hover {
  background-color: #008cba;
}
#metamask-info th {
  padding-top: 2px;
  padding-bottom: 2px;
  text-align: center;
  color: white;
}
</style>
