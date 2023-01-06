<template>
  <div id="app">
  
    <div class="mt-5 mb-5 container-sm shadow-lg p-3 rounded bg-white">
      <div class="alert alert-success d-flex justify-content-center">
        <h1>Live exchange Rate</h1>
      </div>
      <div v-if="this.data" class="alert alert-warning d-flex justify-content-center">
        <div>
          <p v-if="this.data.date">{{ this.data.date }}</p>
          <p v-else>Right now changes are:</p>
        </div>
      </div>
      <div v-if="error" class="alert alert-danger d-flex justify-content-center">
        {{ error }}
      </div>
      <div class="">
        <DateInput @getApiData="(date) => getData(date)" />
      </div>
      <div class="mt-3">
        <ul class="list-group m-4">
          <template v-if="this.data">
            <div v-for="(value, key, index) in this.data" :key="key"
              class="row mt-1 mb-1 border border-success rounded-3">
              <template v-if="key !== 'date'">
                <div class="col-1 mt-2">{{ index + 1}}</div>
                <li class="col-sm list-group-item border-0">
                  <ShowRate class="w-100" state="sell" :price="value.sell" :country="key" />
                </li>
                <li class="col-sm list-group-item border-0">
                  <ShowRate class="w-100" state="buy" :price="value.buy" :country="key" />
                </li>
              </template>
            </div>
          </template>
          <template v-else>
            <div class="d-flex justify-content-center">Loading...</div>
          </template>
        </ul>
      </div>
    </div>
  
  </div>
</template>

<script>
import DateInput from './components/DateInput.vue';
import { apiUrl } from './config.js';
import axios from 'axios';
import ShowRate from './components/ShowRate.vue';
import { apiUrlDate } from './config.js';

export default {
  name: 'App',
  components: {
    DateInput,
    ShowRate
  },

  data() {
    return {
      data: null,
      error: null
    }
  },

  async mounted() {
    try {
      let apiData = await axios.get(apiUrl());
      this.data = apiData.data;
    } catch (err) {
      console.log(err);
    }
  },

  methods: {
    async getData(date) {
      if (!date) {
        this.error = "No data Provided!";
        console.log("No data!");
        return;
      }

      let inputDate = new Date(date);
      let rightNow = new Date();

      if (inputDate > rightNow) {
        this.error = "Can't predict The data, can I?";
        return;
      }
      this.error = null;
      this.data = null;

      try {
        let myUrl = apiUrlDate(date);
        let res = await axios.get(myUrl);
        this.data = res.data;
      } catch (err) {
        console.log(err);
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    background-color: rgb(135, 135, 135);
    }
    
    html {
      background-color: rgb(135, 135, 135);
      height: 100%;
      margin: 0;
      padding: 0;
    }
    
    .border-success:hover {
      border: 3px solid green !important;
      transition: all .15s !important;
    }
</style>
