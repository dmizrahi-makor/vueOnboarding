<template>
  <v-app
    cols="12"
    class='align-center grey lighten-5 app'
  >
    <v-container
      class="fill-height"
      justify-center
      align-center
    >
      <v-row justify="center">
        <v-col
          class="pa-10 elevation-7"
          cols="12"
          align-self="center"
          md="8"
          elevation="2"
        >
          <h1 class="mb-4"><span>Client</span> <span>Information</span></h1>
          <v-form
            v-model="isValid"
            @submit.prevent='handleSubmit'
          >
            <v-container class="pa-0">
              <v-row>
                <v-col
                  cols="12"
                  md="6"
                >
                  <v-text-field
                    height="10"
                    class="rounded-0 custom-border"
                    v-model="formData.name"
                    :rules="fieldRules"
                    label="Name"
                    required
                    outlined
                    validate-on-blur
                  ></v-text-field>
                </v-col>
                <v-spacer></v-spacer>
                <v-col
                  cols="12"
                  md="6"
                >
                  <v-text-field
                    height="10"
                    class="rounded-0 custom-border"
                    v-model="formData.email"
                    :rules="emailRules"
                    label="Email"
                    required
                    outlined
                    validate-on-blur
                  ></v-text-field>
                </v-col>

              </v-row>
              <v-row>
                <v-col
                  cols="12"
                  md="6"
                >
                  <v-text-field
                    height="10"
                    class="rounded-0 custom-border"
                    v-model="formData.company"
                    :rules="fieldRules"
                    label="Company"
                    required
                    outlined
                    validate-on-blur
                  ></v-text-field>
                </v-col>
                <v-col
                  cols="12"
                  md="6"
                >
                  <v-row no-gutters>
                    <v-col
                      cols="4"
                      md="3"
                    >
                      <v-autocomplete
                        class="rounded-0 custom-border"
                        v-model="formData.extension"
                        :rules="fieldRules"
                        :items="refinedCountries"
                        :filter="customFilter"
                        item-text="code"
                        outlined
                        label="Country"
                        validate-on-blur
                      ></v-autocomplete>
                    </v-col>
                    <v-col
                      cols="8"
                      md="9"
                    >
                      <v-text-field
                        class="rounded-0 custom-border"
                        v-model="formData.number"
                        :rules="phoneRules"
                        label="Number"
                        required
                        outlined
                        validate-on-blur
                      ></v-text-field>
                    </v-col>
                  </v-row>

                </v-col>
              </v-row>
              <v-row>
                <v-col class="d-flex justify-end pt-0">
                  <v-btn
                    class="rounded-0 custom-border"
                    type='submit'
                    large
                    outlined
                  >Send</v-btn>
                </v-col>
              </v-row>
            </v-container>
          </v-form>
        </v-col>
      </v-row>
      <div class="text-center">

        <v-snackbar
          v-model="snackbar"
          timeout="5000"
          outlined
          top
          color="pink"
        >
          Please proceed to the link sent to your email.

          <template v-slot:action="{ attrs }">
            <v-btn
              color="blue"
              text
              v-bind="attrs"
              @click="snackbar = false"
            >
              Close
            </v-btn>
          </template>
        </v-snackbar>
      </div>
    </v-container>
  </v-app>
</template>

<script>
import countries from "./data/countries";

export default {
  name: "App",

  components: {},

  data: () => {
    const refinedCountries = countries.map((country) => ({
      name: country.name,
      code: country.dial_code,
    }));
    const fieldRules = [(value) => value.trim() !== "" || "Field is empty"];
    return {
      isValid: false,
      formData: { name: "", email: "", company: "", extension: "", number: "" },
      refinedCountries,
      snackbar: false,
      fieldRules,
      phoneRules: [
        ...fieldRules,
        (value) => Number.isInteger(Number(value)) || "Not a number",
        (value) => value.trim().length > 6 || "Enter a valid number",
      ],
      emailRules: [
        ...fieldRules,
        (value) => {
          const validEmailRegex = /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/;
          const isEmail = validEmailRegex.test(value);
          return isEmail || "Enter a valid email address";
        },
      ],
    };
  },
  methods: {
    customFilter(item, queryText) {
      console.log("filtering");
      return item.name.toLowerCase().startsWith(queryText.toLowerCase());
    },
    handleSubmit() {
      if (this.isValid) {
        const { name, email, company, extension, number } = this.formData;
        this.snackbar = true;
        console.log("submitted", {
          name,
          email,
          company,
          phoneNumber: `${extension}-${number}`,
        });
      }
      console.log("prob not valid");
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Work+Sans:wght@200;300;400&display=swap");
.app {
  font-family: "Work Sans";
}

.v-input.custom-border.rounded-0 fieldset {
  border: 2px solid gray !important;
  /* transform: scale(2); */
}

button.custom-border {
  border: 2px solid gray;
}
</style>