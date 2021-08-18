<template>
  <div class="text-center m-auto p-10">
    <h1 class="text-3xl mb-3">Proxy Contract Inspector</h1>
    <p class="max-w-xl mx-auto mb-3 text-lg">
      Find the address of the implementation contract currently used by any
      proxy contract on the Ethereum blockchain that implements
      <a target="_blank" href="https://eips.ethereum.org/EIPS/eip-1967"
        >EIP-1967</a
      >.
    </p>
    <form v-on:submit.prevent="onSubmit">
      <input
        type="text"
        v-model="address"
        placeholder="Enter the address of the proxy contract"
        class="text-sm p-2 text-white rounded-sm"
      />
      <input
        type="submit"
        :value="loading ? 'Loading...' : 'Get Address'"
        :disabled="!valid || loading"
        class="
          h-10
          px-5
          m-2
          text-white
          transition-colors
          duration-150
          rounded-sm
          focus:shadow-outline
          cursor-pointer
        "
      />
    </form>

    <div v-if="result" class="mt-8 py-8 border rounded-sm result-box mx-auto">
      The implementation contract address is
      <h2 class="mt-1 mb-4">{{ result }}</h2>
      <div class="text-xs">
        <a target="_blank" :href="`https://etherscan.io/address/${result}`">
          View on Etherscan</a
        >
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <a
          target="_blank"
          :href="`https://dashboard.tenderly.co/contract/mainnet/${result}`"
        >
          View on Tenderly</a
        >
      </div>
    </div>
  </div>
</template>

<script>
import { ethers } from "ethers";

export default {
  name: "Query",
  data() {
    return {
      loading: false,
      address: "",
      result: "",
    };
  },
  computed: {
    valid() {
      return this.address.length == 42 && this.address.startsWith("0x");
    },
  },
  methods: {
    async onSubmit() {
      if (!this.valid) {
        return;
      }
      this.loading = true;

      const slot =
        "0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc"; // https://eips.ethereum.org/EIPS/eip-1967
      try {
        const provider = await ethers.getDefaultProvider();
        const data = await provider.getStorageAt(this.address, slot);
        if (
          data !=
          "0x0000000000000000000000000000000000000000000000000000000000000000"
        ) {
          this.result = "0x" + data.slice(26);
        } else {
          this.result = "";
        }
      } catch {
        this.result = "";
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>