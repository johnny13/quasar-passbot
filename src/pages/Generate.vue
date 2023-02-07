<template>
  <q-page class="q-pa-lg column">
    <h4 class="q-mb-sm q-mt-none">Password Strength Test</h4>
    <q-input
      stack-label
      outlined
      type="text"
      autocomplete="off"
      autofill="off"
      :input-class="{ 'data-lpignore': true }"
      class="q-mb-md"
      label="Type or Paste your password"
      v-model="passwordValue"
    />
    <q-item>
      <password-meter :password="passwordValue" />
    </q-item>

    <div class="spacing-block" style="height: 10px"></div>

    <h4 class="q-mb-md q-mt-lg">Generate String Password</h4>
    <q-input
      stack-label
      outlined
      type="text"
      autocomplete="off"
      :input-class="{ 'data-lpignore': true }"
      v-model="generatedPassword"
      label="Generated password"
    >
      <template v-slot:append>
        <q-btn
          round
          color="primary"
          dense
          icon="difference"
          @click="copyGeneratedPassword"
        />
      </template>
    </q-input>
    <q-item>
      <q-item-section>
        <q-slider
          snap
          label
          markers
          :step="12"
          :min="0"
          :max="48"
          :color="strengthColor"
          :label-value="strengthLabel"
          v-model="strengthLevel"
        />
      </q-item-section>
    </q-item>

    <q-item class="q-mt-md q-mb-lg">
      <h2>How It Works</h2>
      <hr />
      <dl class="dl-horizontal">
        <dt>Characters<br /><small>Is Optional</small></dt>
        <dd>
          The <strong>characters</strong> attribute is used to set the character
          type used in the password. You can use numbers <code>0-9</code>,
          letters <code>a-z</code> (and or) <code>A-Z</code>, and spical
          characters <code>#</code>. All of these sets can be used individually,
          but they all work together too.
        </dd>
        <br />

        <dt>Size<br /><small>Is Optional</small></dt>
        <dd>
          The <strong>size</strong> attribute is used to set the number of
          characters that is used in the field, if you need a password with 12
          characters, then set this attribute to 12.
        </dd>
        <br />

        <dt>Type<br /><small>Is Optional</small></dt>
        <dd>
          The <strong>type</strong> attribute is used to change the field type
          from <code>text</code> to <code>password</code>.
        </dd>
        <br />

        <dt>Placeholder<br /><small>Is Optional</small></dt>
        <dd>
          The <strong>placeholder</strong> attribute is used to set the place
          holder text on the field input.
        </dd>
        <br />

        <dt>Auto<br /><small>Is Optional</small></dt>
        <dd>
          The <strong>auto</strong> attribute is used to suppress the automatic
          generation of the password on load, if you don't want the field to
          automatically generate the password on load, then pass
          <code>false</code> to the component.
        </dd>
      </dl>
    </q-item>

    <div class="spacing-block" style="height: 50px"></div>

    <q-item>
      <PasswordGenerator />
    </q-item>

    <div class="spacing-block" style="height: 50px"></div>

    <q-card class="bg-grey-8 text-white q-mb-large">
      <q-card-section class="text-h6 q-pa-sm">
        <div class="text-h6">Password Tester</div>
      </q-card-section>
      <q-card-section class="q-pa-sm row">
        <q-item class="col-lg-4 col-md-4 col-sm-12 col-xs-12">
          <q-item-section> Current Password </q-item-section>
        </q-item>
        <q-item class="col-lg-8 col-md-8 col-sm-12 col-xs-12">
          <q-item-section>
            <q-input
              type="password"
              dark
              dense
              outlined
              color="white"
              round
              v-model="password_dict.current_password"
              label="Current Password"
            />
          </q-item-section>
        </q-item>
        <q-item class="col-lg-4 col-md-4 col-sm-12 col-xs-12">
          <q-item-section> New Password </q-item-section>
        </q-item>
        <q-item class="col-lg-8 col-md-8 col-sm-12 col-xs-12">
          <q-item-section>
            <q-input
              stack-label
              outlined
              type="text"
              autocomplete="off"
              color="primary"
              v-model="password_dict.new_password"
              label="New Password"
            />
          </q-item-section>
        </q-item>
        <q-item class="col-lg-4 col-md-4 col-sm-12 col-xs-12">
          <q-item-section> Confirm New Password </q-item-section>
        </q-item>
        <q-item class="col-lg-8 col-md-8 col-sm-12 col-xs-12">
          <q-item-section>
            <q-input
              stack-label
              outlined
              type="text"
              autocomplete="off"
              color="primary"
              v-model="password_dict.confirm_new_password"
              label="Confirm New Password"
            />
          </q-item-section>
        </q-item>
      </q-card-section>
      <q-card-actions align="right">
        <q-btn class="text-capitalize bg-info text-white"
          >Change Password</q-btn
        >
      </q-card-actions>
    </q-card>
  </q-page>
</template>

<script>
//import { defineComponent } from "vue";
import { copyToClipboard, useQuasar } from "quasar";
import passwordMeter from "vue-simple-password-meter";
import PasswordGenerator from "components/PasswordGenerator";

export default {
  name: "GeneratePage",
  newPassword: "",
  components: { passwordMeter, PasswordGenerator },
  setup() {
    const $q = useQuasar();

    return {
      triggerPositive() {
        $q.notify({
          type: "positive",
          message: "Password Copied to your Clipboard!",
        });
      },
    };
  },
  methods: {
    copyGeneratedPassword() {
      this.newPassword = this.generatedPassword;
      console.log("copy:" + this.newPassword);

      copyToClipboard(this.generatedPassword);

      this.triggerPositive();
    },
  },
  data() {
    return {
      strengthLevel: 24,
      passwordValue: null,
      user_details: {},
      password_dict: {
        current_password: "",
        new_password: "",
      },
    };
  },
  computed: {
    generatedPassword() {
      let charactersArray = "a-z".split(",");
      let CharacterSet = "";
      let password = "";
      let size = 8;

      switch (this.strengthLevel) {
        case 12:
          size = 10;
          charactersArray = "a-z,A-Z".split(",");
          break;
        case 24:
          size = 12;
          charactersArray = "a-z,A-Z,0-9".split(",");
          break;
        case 36:
          size = 14;
          charactersArray = "a-z,A-Z,0-9,#".split(",");
          break;
        case 48:
          size = 16;
          charactersArray = "a-z,A-Z,0-9,#".split(",");
          break;
      }
      if (charactersArray.indexOf("a-z") >= 0) {
        CharacterSet += "abcdefghijklmnopqrstuvwxyz";
      }
      if (charactersArray.indexOf("A-Z") >= 0) {
        CharacterSet += "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
      }

      if (charactersArray.indexOf("0-9") >= 0) {
        CharacterSet += "0123456789";
      }
      if (charactersArray.indexOf("#") >= 0) {
        CharacterSet += "![]{}()%&*$#^<>~@|";
      }
      for (let i = 0; i < size; i++) {
        password += CharacterSet.charAt(
          Math.floor(Math.random() * CharacterSet.length)
        );
      }

      return password;
    },
    strengthLabel() {
      switch (this.strengthLevel) {
        case 12:
          return "poor";
        case 24:
          return "fair";
        case 36:
          return "good";
        case 48:
          return "excellent";
        default:
          return "weak";
      }
    },
    strengthColor() {
      switch (this.strengthLevel) {
        case 12:
          return "amber-10";
        case 24:
          return "orange-6";
        case 36:
          return "primary";
        case 48:
          return "positive";
        default:
          return "blue-grey-3";
      }
    },
  },
};
</script>

<style lang="scss">
.vue-progress-path path {
  stroke-width: 2 !important;
}

.spacing-block {
  width: 100%;
  display: block;
  position: relative;
  margin: 0;
  padding: 0;
  clear: both;
}

$orange: #fb964d;
$yellgrn: #b0dc53;

.po-password-strength-bar {
  border-radius: 5px;
  transition: all 0.4s linear;
  height: 10px;
  margin-top: 8px;
  background: #d4d4d4;
  border-bottom: 1px solid #cfcfcf;
  width: 100%;
}
.po-password-strength-bar.risky {
  background-color: $negative;
  width: 10%;
}
.po-password-strength-bar.guessable {
  background-color: $orange;
  width: 32.5%;
}
.po-password-strength-bar.weak {
  background-color: $warning;
  width: 55%;
}
.po-password-strength-bar.safe {
  background-color: $yellgrn;
  width: 77.5%;
}
.po-password-strength-bar.secure {
  background-color: $positive;
  width: 100%;
}
</style>
