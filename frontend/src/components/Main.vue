<template>
  <div class="container">
    <div class="col-md-12 row">
      <div class="col-md-3"></div>
      <div class="col-md-6">
        <b-alert variant="success" :show="generated">
          <strong>Your short url is {{ short_url }}</strong>
        </b-alert>

        <form @submit.prevent="submit">
          <div
            class="input-group input-group-lg"
            :class="{ 'form-group--error': $v.long_url.$error }"
          >
            <span class="input-group-text" id="inputGroup-sizing-sm"
              >Long URL</span
            >
            <input
              class="form-control"
              v-model="long_url"
              v-model.trim="$v.long_url.$model"
            />
          </div>
          <br />
          <p class="text-start text-danger" v-if="!$v.long_url.required">
            Url is required
          </p>
          <p class="text-start text-danger" v-if="!$v.long_url.url">
            You must enter a valid url
          </p>
          <button class="btn btn-outline-primary float-end" :disabled="$v.$invalid" type="submit">
            Generate Short Url
          </button>
        </form>
      </div>
      <div class="col-md-3"></div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { required, url } from "vuelidate/lib/validators";

export default {
  data() {
    return {
      long_url: "",
      short_url: "",
      generated: false,
    };
  },
  validations: {
    long_url: {
      required,
      url,
    },
  },
  methods: {
    submit() {
      this.$v.$touch();
      if (this.$v.$invalid) {
        return;
      }
      const base_url = `${this.getBaseUrl()}:3000`;
      axios
        .post(
          `${base_url}/api/urls`,
          { url: this.long_url },
          { headers: { "Content-Type": "application/json" } }
        )
        .then((res) => {
          const data = res.data;
          this.short_url = data.short_url;
          this.long_url = data.original_url;
          this.generated = true;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    getBaseUrl(){
      return process.env.DOMAIN_NAME? process.env.DOMAIN_NAME: `${window.location.protocol}//${window.location.hostname}`
    }
  },
};
</script>