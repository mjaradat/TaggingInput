<template>
  <div class="publisher-actions" >
    <div class="publisher-tools mr-auto">
      <span
        v-for="(item, index) in items"
        :key="index"
        class="badge badge-pill badge-subtle border m-1"
        :class="{ 'badge-success': !item.isNew,
                    'badge-warning': item.isNew,
                    'badge-danger': item.isInvalid}"
      >
        <span class="text-key pl-1">{{ item[textKey] }}</span>
        <div class="has-clearable d-inline pl-4">
          <button type="button" class="close show p-0" aria-label="Close" @click="removeItem(index)">
            <span aria-hidden="true">
              <i class="fa fa-times-circle"></i>
            </span>
          </button>
        </div>
      </span>
      <em v-if="!items.length">E.g.</em>
      <span
        v-if="!items.length"
        class="badge badge-pill badge-subtle border m-1 badge-dark disabled"
        aria-disabled
      ><em>{{ textKey.charAt(0).toUpperCase() + textKey.substring(1) }}</em></span>
    </div>
  </div>
</template>



<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  props: {
    items: {
      type: Array,
      required: true
    },
    textKey: {
      type: String,
      required: true
    }
  },
  methods: {
    removeItem(itemIndex: number) {
      this.items.splice(itemIndex, 1);
    }
  }
});
</script>
