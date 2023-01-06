<template>
  <div class="row border w-25 border-o rounded-3">
    <div class=" col-2">{{ state }}</div>
    <div class="col-3">{{ country }}</div>
    <div class="col-6">{{ price }} T</div>
  </div>
</template>

<script>
export default {
  name: 'ShowRate',
  props: ['state', 'price', 'country', 'flag']
}
</script>

<style>
.border-o:hover {
  border: 3px solid orange !important;
  transition: all .15s !important;
}
</style>
