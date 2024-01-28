<script>
// 把需要的語系 驗證 驗證規則引入

import * as VeeValidate from "vee-validate";
import * as VeeValidateI18n from "@vee-validate/i18n";
import * as VeeValidateRules from "@vee-validate/rules";

// 中文語系 JSON
import zh_TW from "../src/assets/zh_TW.json";
console.log(VeeValidateRules);
// 表單驗證規則全部引入使用
Object.keys(VeeValidateRules).forEach((rule) => {
  VeeValidate.defineRule(rule, VeeValidateRules[rule]);
});

// 讀取外部的資源
VeeValidateI18n.localize({
  zh_TW,
});
// Activate the locale
VeeValidate.configure({
  generateMessage: VeeValidateI18n.localize("zh_TW"),
  validateOnInput: true, // 調整為：輸入文字時，就立即進行驗證
});

export default {
  components: {
    VForm: VeeValidate.Form,
    VField: VeeValidate.Field,
    ErrorMessage: VeeValidate.ErrorMessage,
  },
  data() {
    return {
      user: {
        email: "",
        name: "",
        phone: "",
        address: "",
      },
    };
  },
  methods: {
    onSubmit(value) {
      console.log(value);
    },

    // 電話驗證規則
    isPhone(value) {
      const phoneNumber = /^(09)[0-9]{8}$/;
      return phoneNumber.test(value) ? true : "需要正確的電話號碼";
    },
  },
};
</script>

<template>
  <main class="d-flex flex-column justify-content-center align-items-center">
    <v-form class="vForm" v-slot="{ errors }" @submit="onSubmit">
      {{ errors }}
      <v-field
        id="email"
        name="email"
        type="email"
        class="form-control"
        :class="{ 'is-invalid': errors['email'] }"
        placeholder="請輸入 Email"
        rules="email|required"
        v-model="user.email"
      ></v-field>
      <error-message name="email" class="invalid-feedback"></error-message>

      <v-field
        id="name"
        name="name"
        type="text"
        class="form-control"
        :class="{ 'is-invalid': errors['name'] }"
        placeholder="請輸入姓名"
        rules="required"
        v-model="user.name"
      ></v-field>
      <error-message name="name" class="invalid-feedback"></error-message>

      <v-field
        id="phone"
        name="phone"
        type="text"
        class="form-control"
        :class="{ 'is-invalid': errors['phone'] }"
        placeholder="請輸入電話"
        :rules="isPhone"
        v-model="user.phone"
      ></v-field>
      <error-message name="phone" class="invalid-feedback"></error-message>

      <v-field
        id="address"
        name="address"
        type="text"
        class="form-control"
        :class="{ 'is-invalid': errors['address'] }"
        placeholder="請輸入地址"
        rules="required"
        v-model="user.address"
      ></v-field>
      <error-message name="address" class="invalid-feedback"></error-message>

      <button class="btn btn-primary" type="submit" @click="onSubmit">
        Submit
      </button>
    </v-form>

    {{ user }}
  </main>
</template>

<style lang="scss" scoped>
main {
  height: 100vh;
  .vForm {
    width: 500px;
  }
}
</style>
