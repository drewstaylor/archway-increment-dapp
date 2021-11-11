<template>
  <img alt="logo" src="./assets/logo.svg">
</template>

<script>
import { QueryClient, setupAuthExtension } from "@cosmjs/stargate";
import { Tendermint34Client } from "@cosmjs/tendermint-rpc";

const RPC = "https://rpc.constantine-1.archway.tech:443";
const ContractAddress = "archway1vw9za2nv564rymxdunj5frwwuegpnv90uf6duw";

const Alice = {
  mnemonic: "enlist hip relief stomach skate base shallow young switch frequent cry park",
  address0: "arhcway14qemq0vw6y3gc3u3e0aty2e764u4gs5lndxgyk",
  address1: "archway1hhg2rlu9jscacku2wwckws7932qqqu8xm5ca8y",
  address2: "archway1xv9tklw7d82sezh9haa573wufgy59vmwnxhnsl",
  address3: "archway17yg9mssjenmc3jkqth6ulcwj9cxujrxxg9nmzk",
  address4: "archway1f7j7ryulwjfe9ljplvhtcaxa6wqgula3nh873j",
};

export default {
  name: "App",
  data: () => ({
    contract: ContractAddress,
    counter: null,
    queryClient: null,
    rpc: RPC,
    tendermintClient: null,
    user: Alice
  }),
  mounted: async function () {
    await this.init();
  },
  methods: {
    init: async function () {
      this.tendermintClient = await Tendermint34Client.connect(this.rpc);
      this.queryClient = QueryClient.withExtensions(this.tendermintClient, setupAuthExtension);
      console.log('queryClient', this.queryClient);
    },
    getCount: async function () {
      
    },
    incrementCounter: async function () {

    },
    resetCounter: async function () {

    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
