# vue-otp-input

## How to Install

```
npm install vue-otp-input --save
```

### codesandbox example

https://codesandbox.io/s/x72or1nyvw

### How to use it

```
<template>
  <div>
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
```

### Things remaingin

1. Add CSS classes for disabled and error