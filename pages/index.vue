<template>
  <div class="d-flex align-center justify-center" style="height: 100vh">
    <v-container fluid class="text-center">
      <v-row justify="center">
        <v-col cols="12" sm="8" md="6" lg="4">
          <v-card>
            <v-card-title class="text-center">Login</v-card-title>
            <v-card-text>
              <v-form>
                <v-text-field
                  v-model="data.login.email"
                  label="Email"
                  outlined
                ></v-text-field>
                <v-text-field
                  v-model="data.login.password"
                  label="Password"
                  outlined
                  type="password"
                ></v-text-field>
              </v-form>
            </v-card-text>
            <v-card-actions class="d-flex justify-end align-center">
              <v-btn class="mr-2 mb-2" color="primary" @click="userLogin">Login</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
    <v-snackbar
      :value="requestStatus"
      timeout="3000"
      absolute
      top
      right
      :color="requestStatus ? 'success' : 'red'"
    >
      {{ response.message }}
    </v-snackbar>
  </div>
</template>

<script lang="ts">
// region Imports
import axios from "axios";

import {LoginRequest} from "~/models/api/request_models/login_request";
import {LoginResponse} from "~/models/api/response_models/login_response";
// endregion
export default {
  data() {
    return {
      data: new LoginRequest(),
      requestStatus: false,
      response: new LoginResponse()
    }
  },
  mounted() {
    sessionStorage.clear();
  },
  methods: {
    /*
      Sends LoginRequest model to Search and Stay API and receives
      the LoginResponse data
    */
    async userLogin() {
      const result = await axios.post(
        "https://sys-dev.searchandstay.com/api/admin/login_json",
        this.data
      );
      this.response = result.data;

      console.log(this.response);

      if (this.response.success) {
        this.requestStatus = this.response.success;
        sessionStorage.setItem("accessToken", this.response.data?.result.access_token);
        await this.$router.push("/home")
      }
    }
  }
}
</script>
