<template>
  <validation-observer ref="observer" v-slot="{ invalid }">
    <div>
      <v-row>
        <v-col align="center" justify="center">
          <v-card style="width: 40%; margin-top: 2.5%;">
            <v-card-title justify="center">
              <v-col>
                <h2>Log in</h2>
              </v-col>
            </v-card-title>

            <b-container v-if="error">
              <b-alert
                show
                variant="danger"
                class="d-flex justify-content-center"
                >{{ errormessage }}</b-alert
              >
            </b-container>

            <form @submit.prevent="submit">
              <validation-provider v-slot="{ errors }" name="Username">
                <v-text-field
                  style="width: 400px"
                  v-model="form.email"
                  label="Username"
                  :error-messages="errors"
                  required
                ></v-text-field>
              </validation-provider>

              <validation-provider v-slot="{ errors }" name="Password">
                <v-text-field
                  v-model="form.password"
                  style="width: 400px;"
                  :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                  :type="show1 ? 'text' : 'password'"
                  :error-messages="errors"
                  name="password"
                  label="Password"
                  required
                  @click:append="show1 = !show1"
                ></v-text-field>
              </validation-provider>

              <br>

              <v-btn
                class="btn btn-outline-primary btn-block z-depth-2"
                style="width: 400px; color: primary"
                type="submit"
                :disabled="invalid"
                outlined
              >
                Log in
              </v-btn>

              <br />
              <br />
            </form>
          </v-card>
        </v-col>
      </v-row>
    </div>
  </validation-observer>
</template>

<script>
import { required } from "vee-validate/dist/rules";
import {
  extend,
  ValidationObserver,
  ValidationProvider,
  setInteractionMode,
} from "vee-validate";
import axios from "axios";
import { mapGetters, mapMutations } from "vuex";

setInteractionMode("eager");

extend("required", {
  ...required,
  message: "All fields must be filled.",
});

export default {
  components: {
    ValidationProvider,
    ValidationObserver,
  },
  data() {
    return {
      form: {
        password: "",
        email: "",
      },
      show1: false,
      rules: {
        min: (v) => v.length >= 8 || "Minimum 8 karaktera.",
      },
      error: false,
      errormessage: ""
    };
  },
  methods: {
    ...mapMutations(["setUser"]),
    submit() {
      this.$refs.observer.validate();
      axios
        .post("api/auth/login", this.form)
        .then((user) => {
          this.setUser(user.data);
          this.$router.push("/index");
        })
        .catch((error) => {
          console.log(error);
          this.error = true;
          this.errormessage = "Bad credentials.";
        });
    },
  },
};
</script>

<style>
</style>