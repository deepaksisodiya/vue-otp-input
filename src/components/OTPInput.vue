<template>
  <div class="otp-container">
    <span v-for="index in numberOfInput" :key="index">
      <input
        type="number"
        v-model="otps[index - 1]"
        :ref="'otpDigit' + index"
        @keypress="onKeyPress"
        @keyup.right="focusInputByRef('otpDigit' + (index + 1))"
        @keyup.left="focusInputByRef('otpDigit' + (index - 1))"
        @keyup.delete="focusInputByRef('otpDigit' + (index - 1))"
        :style="inputStyle"
        :disabled="isDisabled"
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
      required: false,
      default: false
    },
    isError: {
      type: Boolean,
      required: false,
      default: false
    },
    shouldResetOTP: {
      type: Boolean,
      required: false,
      default: false
    },
    numberOfInput: {
      type: Number,
      required: false,
      default: 4
    },
    otpInputStyle: {
      type: Object,
      required: false,
      default: function() {
        return {
          width: "40px",
          height: "40px",
          "margin-right": "10px",
          "text-align": "center",
          "font-size": "20px",
          appearance: "none",
          "-webkit-appearance": "none",
          "border-radius": "2px"
        };
      }
    },
    error: {
      type: Object,
      required: false,
      default: function() {
        return {
          "border-color": "red"
        };
      }
    },
    onChangeOTP: {
      type: Function,
      required: true
    }
  },

  data() {
    return {
      otps: []
    };
  },

  watch: {
    shouldResetOTP: function(newProp, oldProp) {
      if (newProp === !oldProp) {
        this.resetOTPInput();
      }
    }
  },

  computed: {
    otp() {
      const otp = this.otps.join("");
      if (otp) return parseInt(otp, 10);
      return null;
    },
    inputStyle() {
      if (this.isError) {
        return {
          ...this.otpInputStyle,
          ...this.error
        };
      }

      return {
        ...this.otpInputStyle
      };
    }
  },

  methods: {
    onKeyPress(event) {
      if (event.target.value.length === 1) return event.preventDefault();
    },

    focusInputByRef(ref) {
      if (!this.$refs[ref]) return;
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

      this.otps = arrayOfNumbers;

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
