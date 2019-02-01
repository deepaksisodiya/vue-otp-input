<template>
  <div class="otp-container">
    <span v-for="index in 6" :key="index">
      <input
        type="number"
        v-model="otps[index]"
        class="otp-number-input"
        :ref="'otpDigit' + index"
        @keypress="onKeyPress"
        @keyup.right="moveToNextInput('otpDigit' + (index + 1))"
        @keyup.left="moveToPreviousInput('otpDigit' + (index - 1))"
        @keyup.delete="moveToPreviousInput('otpDigit' + (index - 1))"
        :class="{ error: isError }"
        :isDisabled="isDisabled"
        @paste="onPaste"
        @input="onInput"
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
    }
  },

  data() {
    return {
      otpDigit1: null,
      otpDigit2: null,
      otpDigit3: null,
      otpDigit4: null,
      otpDigit5: null,
      otpDigit6: null,
      otps: []
    };
  },

  computed: {
    code() {
      const code =
        "" +
        this.otpDigit1 +
        this.otpDigit2 +
        this.otpDigit3 +
        this.otpDigit4 +
        this.otpDigit5 +
        this.otpDigit6;
      return parseInt(code, 10);
    }
  },

  updated() {
    if (this.shouldResetOTP) this.shouldResetOTPInput();
  },

  methods: {
    onKeyPress(event) {
      if (event.target.value.length === 1) return event.preventDefault();
    },

    moveToNextInput(ref) {
      this.$refs[ref][0].focus();
    },

    moveToPreviousInput(ref) {
      this.$refs[ref][0].focus();
    },

    onInput(event) {
      if (event.inputType === "deleteContentBackward") return false;

      this.onChangeOTP(this.code);
      this.moveToNextInput(event);
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

      // looping and setting codes
      arrayOfNumbers.forEach((number, index) => {
        const key = `code${index + 1}`;
        this[key] = null;
        this[key] = number;
      });

      // focus the last input element according to length
      const focusKey = `code${arrayOfNumbers.length}`;
      this.$refs[focusKey].focus();

      event.preventDefault();
    },

    shouldResetOTPInput() {
      this.otpDigit1 = null;
      this.otpDigit2 = null;
      this.otpDigit3 = null;
      this.otpDigit4 = null;
      this.otpDigit5 = null;
      this.otpDigit6 = null;
    }
  }
};
</script>
