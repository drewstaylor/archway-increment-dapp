<template>
  <img alt="logo" src="./assets/logo.svg">
  <div class="content">
    <div class="status-display" v-if="!isNaN(counter)">
      <ul class="status">
        <li class="counter"><strong>Counter:</strong>&nbsp;{{counter}}</li>
      </ul>
    </div>
    <div class="button-controls">
      <button id="incrementer" class="btn btn-main">Increment Counter</button>
      <button id="resetter" class="btn btn-alt">Reset Counter</button>
    </div>
  </div>
</template>

<script>
import { DirectSecp256k1HdWallet } from "@cosmjs/proto-signing";
import { SigningCosmWasmClient } from "@cosmjs/cosmwasm-stargate";

const RPC = "https://rpc.constantine-1.archway.tech:443";
const ContractAddress = "archway1vw9za2nv564rymxdunj5frwwuegpnv90uf6duw";
const BECH32_PREFIX = "archway";

export default {
  name: "App",
  data: () => ({
    contract: ContractAddress,
    counter: null,
    cwClient: null,
    handlers: {
      query: null,
      tx: null
    },
    loading: false,
    queryClient: null,
    rpc: RPC,
    tendermintClient: null,
    user: null
  }),
  mounted: async function () {
    // Init query/tx client
    await this.init();

    // Get count
    let counter = await this.getCount();
    if (!isNaN(counter.count)) {
      this.counter = counter.count;
    } else {
      console.warn('Error expected numeric value from counter, found: ', typeof counter.count);
    }
  },
  methods: {
    /**
     * Aliases 2 handlers for querying (reading from chain) and transacting (writing to chain)
     * @see {File} ./.env
     * @see {File} ./env.example
     * @see https://cli.vuejs.org/guide/mode-and-env.html#environment-variables
     */
    init: async function () {
      const mnemonic = process.env.VUE_APP_ACCOUNT_MNEMONIC;
      this.user = await DirectSecp256k1HdWallet.fromMnemonic(mnemonic, { prefix: BECH32_PREFIX });
      this.cwClient = await SigningCosmWasmClient.connectWithSigner(this.rpc, this.user);
      this.handlers.tx = this.cwClient.execute;
      this.handlers.query = this.cwClient.queryClient.wasm.queryContractSmart;
      console.log('dApp Initialized', {
        user: this.user,
        client: this.cwClient,
        handlers: this.handlers
      });
    },
    /**
     * Query contract counter
     * @see {SigningCosmWasmClient} this.handlers.query
     * @see https://github.com/drewstaylor/archway-template/blob/main/src/contract.rs#L66-L71
     */
    getCount: async function () {
      // query: async (address, query)
      let entrypoint = {
        get_count: {}
      };
      let query = await this.handlers.query(this.contract, entrypoint);
      console.log('Counter Queried', query);
      return query;
    },
    /**
     * Increment the counter
     * @see {SigningCosmWasmClient} this.handlers.tx
     * @see https://github.com/drewstaylor/archway-template/blob/main/src/contract.rs#L42
     */
    incrementCounter: async function () {
      // execute: async (senderAddress, contractAddress, msg, fee, memo = "", funds)

    },
    /**
     * Reset the counter to 0
     * @see {SigningCosmWasmClient} this.handlers.tx
     * @see https://github.com/drewstaylor/archway-template/blob/main/src/contract.rs#L43
     */
    resetCounter: async function () {
      // execute: async (senderAddress, contractAddress, msg, fee, memo = "", funds)
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
div.content {
  text-align: left;
  margin: auto;
  max-width: 90vw;
}
div.content ul {
  list-style: none;
}
button {
  margin: 1rem;
  padding: 0.25rem;
}
</style>
