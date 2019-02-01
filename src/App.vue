<template>
  <div id="app">
    <OTPInput
      :isDisabled="isDisabled"
      :isError="isError"
      :onChangeOTP="onChangeOTP"
      :shouldResetOTP="shouldResetOTP"
      :numberOfInput="6"
    />
  </div>
</template>

<script>
import OTPInput from "./components/OTPInput.vue";

export default {
  name: "app",

  components: {
    OTPInput
  },

  data() {
    return {
      otp: null,
      isDisabled: false,
      isError: false,
      shouldResetOTP: false
    };
  },

  methods: {
    onChangeOTP(otp) {
      this.otp = otp;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
