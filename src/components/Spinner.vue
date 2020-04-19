<template>
  <div v-if="canShow" class="col-lg-12 text-center">
    <div class="spinner-grow text-primary" role="status">
      <span class="sr-only">Loading...</span>
    </div>
    <div class="spinner-grow text-success" role="status">
      <span class="sr-only">Loading...</span>
    </div>
    <div class="spinner-grow text-warning" role="status">
      <span class="sr-only">Loading...</span>
    </div>
  </div>
</template>


<script lang="ts">
import Vue, { PropType } from "vue";


export default Vue.extend({
  props: {
      canShow: {
          type: Boolean,
          required: true
      }
  }
});
</script>