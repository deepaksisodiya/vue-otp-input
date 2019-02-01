<template>
  <div class="otp-container">
    <input
      type="number"
      v-model="otpDigit1"
      class="otp-number-input"
      ref="otpDigit1"
      @keypress="onKeyPress"
      @keyup.right="moveToNextInput"
      :class="{ error: isError }"
      :isDisabled="isDisabled"
      @paste="onPaste"
      @input="onInput"
    />
    <input
      type="number"
      v-model="otpDigit2"
      class="otp-number-input"
      ref="otpDigit2"
      @keypress="onKeyPress"
      @keyup.right="moveToNextInput"
      @keyup.left="moveToPreviousInput"
      @keyup.delete="moveToPreviousInput"
      :isDisabled="isDisabled"
      :class="{ error: isError }"
      @paste="onPaste"
      @input="onInput"
    />
    <input
      type="number"
      v-model="otpDigit3"
      class="otp-number-input"
      ref="otpDigit3"
      @keypress="onKeyPress"
      @keyup.right="moveToNextInput"
      @keyup.left="moveToPreviousInput"
      @keyup.delete="moveToPreviousInput"
      :isDisabled="isDisabled"
      :class="{ error: isError }"
      @paste="onPaste"
      @input="onInput"
    />
    <input
      type="number"
      v-model="otpDigit4"
      class="otp-number-input"
      ref="otpDigit4"
      @keypress="onKeyPress"
      @keyup.right="moveToNextInput"
      @keyup.left="moveToPreviousInput"
      @keyup.delete="moveToPreviousInput"
      :isDisabled="isDisabled"
      :class="{ error: isError }"
      @paste="onPaste"
      @input="onInput"
    />
    <input
      type="number"
      v-model="otpDigit5"
      class="otp-number-input"
      ref="otpDigit5"
      @keypress="onKeyPress"
      @keyup.right="moveToNextInput"
      @keyup.left="moveToPreviousInput"
      @keyup.delete="moveToPreviousInput"
      :isDisabled="isDisabled"
      :class="{ error: isError }"
      @paste="onPaste"
      @input="onInput"
    />
    <input
      type="number"
      v-model="otpDigit6"
      class="otp-number-input"
      ref="otpDigit6"
      @keypress="onKeyPress"
      @keyup.left="moveToPreviousInput"
      @keyup.delete="moveToPreviousInput"
      :isDisabled="isDisabled"
      :class="{ error: isError }"
      @paste="onPaste"
      @input="onInput"
    />
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
      otpDigit6: null
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

    moveToNextInput(event) {
      if (!event.target.nextElementSibling) return;

      event.target.nextElementSibling.focus();
      event.preventDefault();
    },

    moveToPreviousInput(event) {
      if (!event.target.previousElementSibling) return;

      event.target.previousElementSibling.focus();
      event.preventDefault();
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
