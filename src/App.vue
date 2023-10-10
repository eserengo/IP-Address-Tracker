<template>
  <header>
    <form @submit.prevent="handleSubmit" class="form">
      <label for="inputText" class="label">
        IP Address Tracker
      </label>
      <section class="section">
        <input
          v-model="ipValue"
          type="text"
          id="inputText"
          class="input"
          placeholder="Search for any IP address"
          autocomplete="off"
          autocorrect="off"
          spellcheck="false"
          size="30"
          required />
        <button type="submit" class="btn">
          <img src="./assets/images/icon-arrow.svg" alt="arrow icon" >
        </button>
      </section>
      <p class="error" v-if="hasError" >This is not a valid IP address</p>
    </form>
  </header>

  <DisplayInfo :value="ipValue" v-if="isValidated" />
</template>

<script>
import DisplayInfo from "./components/DisplayInfo.vue"

export default {
  name: "App",

  data() {
    return {
      ipValue: "",
      isValidated: false,
      hasError: false,
    }
  },

  components: {
    DisplayInfo,
  },

  methods: {
    handleSubmit() {
      if (/^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/.test(this.ipValue)) {
        this.isValidated = true;
        this.hasError = false;
      } else {
        this.isValidated = false;
        this.hasError = true;
      }
    }
  }
}
</script>

<style lang="scss">
@import "main.scss";
</style>