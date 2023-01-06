<template>
  <div class="row d-flex justify-content-center">
    <div class="col-1"></div>
    <input type="date" class="col-sm form-control" id="date-input" v-model="this.inputData">
    <div class="col-1"></div>
    <button class="btn btn-success col-2 mt-1 mb-1 ml-2" @click="$emit('getApiData', this.inputData)">Show</button>
    <div class="col-1"></div>
  </div>
</template>

<script>
export default {
  name: 'DateInput',

  data() {
    return {
      inputData: ''
    }
  }
}
</script>
