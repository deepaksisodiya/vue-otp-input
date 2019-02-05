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
import OTPInput from "vue-otp-input";

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

### Props

| Prop name  | type | default value | Required Props
| --------   | -------- | --------- | ---------- |
| isDisabled  | Boolean  | false | false
| IsError  | Boolean  | false | false
| onChangeOTP  | Function | | true
| shouldResetOTP | Boolean | false | false
| numberOfInput | Number | 4 | false
| otpInputStyle | Object | | false
| error| Object | | false