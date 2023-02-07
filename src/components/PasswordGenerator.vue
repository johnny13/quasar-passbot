<template>
  <q-container text-center>
    <h1 class="py-5 mx-auto password" v-if="!isLoading">
      {{ generatedPassword }}
    </h1>
    <q-progress-circular
      indeterminate
      color="primary"
      v-else
    ></q-progress-circular>
    <q-row>
      <q-col cols="12" class="custom-width">
        <q-col>
          <q-slider
            v-model="passwordLength"
            thumb-label
            min="1"
            max="30"
          ></q-slider>
        </q-col>
        <q-col>
          <div class="">
            <q-checkbox
              v-model="includeUpperCase"
              label="Includes Uppercase Character"
            ></q-checkbox>
          </div>
        </q-col>
        <q-col>
          <div class="">
            <q-checkbox
              v-model="includeNumber"
              label="Includes Number Character"
            ></q-checkbox>
          </div>
        </q-col>
        <q-col>
          <div class="">
            <q-checkbox
              v-model="includeSymbol"
              label="Includes Symbol Character"
            ></q-checkbox>
          </div>
        </q-col>
      </q-col>
    </q-row>
    <q-btn block dark x-large @click="generatePassword()">Generate</q-btn>
  </q-container>
</template>

<script>
export default {
  data: () => ({
    isLoading: false,
    generatedPassword: "GeneratedPassword",
    passwordLength: 10,
    includeUpperCase: false,
    includeNumber: false,
    includeSymbol: false,
  }),
  methods: {
    generatePassword() {
      if (!this.passwordLength) return;
      this.triggerLoading(true);

      let password = "";
      let characters = "abcdefghijklmnopqrstuvwxyz";
      if (this.includeUpperCase) characters += "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
      if (this.includeNumber) characters += "0123456789";
      if (this.includeSymbol)
        characters += "!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~";

      for (let i = 0; i < this.passwordLength; i++) {
        password += characters.charAt(
          Math.floor(Math.random() * characters.length)
        );
      }

      this.generatedPassword = password;
      this.triggerLoading(false);
    },
    triggerLoading(state) {
      this.isLoading = state;
    },
  },
};
</script>

<style scoped>
.password {
  overflow-wrap: break-word;
}
</style>
