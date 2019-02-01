<template>
  <div class="otp-container">
    <span v-for="index in numberOfInput" :key="index">
      <input
        type="number"
        v-model="otps[index - 1]"
        class="otp-number-input"
        :ref="'otpDigit' + index"
        @keypress="onKeyPress"
        @keyup.right="focusInputByRef('otpDigit' + (index + 1))"
        @keyup.left="focusInputByRef('otpDigit' + (index - 1))"
        @keyup.delete="focusInputByRef('otpDigit' + (index - 1))"
        :class="{ error: isError }"
        :isDisabled="isDisabled"
        @paste="onPaste"
        @input="onInput($event, 'otpDigit' + (index + 1))"
      />
    </span>
  </div>
</template>

<script>
export default {
  name: "OTPInput",

  props: {
    isDisabled: {
      type: Boolean,
      required: true
    },
    isError: {
      type: Boolean,
      required: true
    },
    shouldResetOTP: {
      type: Boolean,
      required: true
    },
    onChangeOTP: {
      type: Function,
      required: true
    },
    numberOfInput: {
      type: Number,
      required: false,
      default: 4
    }
  },

  data() {
    return {
      otps: []
    };
  },

  computed: {
    otp() {
      const otp = this.otps.join("");
      if (otp) return parseInt(otp, 10);
      return null;
    }
  },

  updated() {
    if (this.shouldResetOTP) this.resetOTPInput();
  },

  methods: {
    onKeyPress(event) {
      if (event.target.value.length === 1) return event.preventDefault();
    },

    focusInputByRef(ref) {
      this.$refs[ref][0].focus();
    },

    onInput(event, ref) {
      this.onChangeOTP(this.otp);

      if (event.inputType === "deleteContentBackward") return false;

      this.focusInputByRef(ref);
      event.preventDefault();
    },

    onPaste(event) {
      // Getting copy text
      const clipboardData =
        event.clipboardData ||
        window.clipboardData ||
        event.originalEvent.clipboardData;
      const pastedData = clipboardData.getData("Text");
      const arrayOfNumbers = pastedData.split("");

      // set the length to 6
      if (arrayOfNumbers.length > 6) arrayOfNumbers.slice(0, 6);

      this.opts = arrayOfNumbers;

      // focus the last input element according to length
      const ref = `otpDigit${arrayOfNumbers.length}`;
      this.focusInputByRef(ref);

      event.preventDefault();
    },

    resetOTPInput() {
      this.otps = [];
    }
  }
};
</script>
