<template>
  <div id="app">
    <div class="loading" v-show="loading">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="icon">
        <path
          d="M304 48c0 26.51-21.49 48-48 48s-48-21.49-48-48 21.49-48 48-48 48 21.49 48 48zm-48 368c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48-21.49-48-48-48zm208-208c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48-21.49-48-48-48zM96 256c0-26.51-21.49-48-48-48S0 229.49 0 256s21.49 48 48 48 48-21.49 48-48zm12.922 99.078c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48c0-26.509-21.491-48-48-48zm294.156 0c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48c0-26.509-21.49-48-48-48zM108.922 60.922c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48-21.491-48-48-48z"
        />
      </svg>
    </div>

    <router-view />
  </div>
</template>

<script>
import { mapState } from "vuex";
import Config from "../config";
import { WINDOWSTATE } from "../types";
export default {
  computed: mapState({
    loading: state => state.loading,
    network: state => state.network
  }),
  mounted() {
    chrome.runtime.sendMessage(
      { action: "GET_EXTENSION_STATE" },
      ({ state } = {}) => {
        if (!state) return false;
        if (state.status === WINDOWSTATE.LOGIN) {
          this.$router.push("/login");
        } else if (state.status === WINDOWSTATE.APPROVE) {
          this.$router.push("/approve");
        }
        chrome.runtime.sendMessage({ action: "RESET_WINDOW_STATE" });
      }
    );
    if (this.network.name === "")
      this.$store.commit("network/change", Config.networks[0]);
  }
};
</script>

<style>
@import url("css/normalize.css");
@import url("css/style.css");
</style>
