<template>
  <div>
    <div class="publisher-actions border">
      <div class="publisher-tools mr-auto">
        <span
          v-for="(item, index) in items"
          :key="index"
          class="badge badge-pill border m-1"
          :class="{ 'badge-success': !item.isNew,
                    'badge-warning ': item.isNew,
                    'badge-danger': !item.isValid}"
          v-show="index < limit"
        >
          <i v-if="item.isValid && !item.isNew && !item.group" title="Correct" class="fa fa-check-circle fa-fw"></i>
          <i v-if="item.isValid && !item.isNew && item.group" title="Double click to expand" class="fa fa-users fa-fw"></i>

          <i v-else-if="item.isValid && item.isNew" title="New Item" class="fa fa-exclamation-circle fa-fw"></i>
          <i
            v-else-if="!item.isValid"
            data-toggle="tooltip"
            data-placement="bottom"
            data-original-title="Tooltip on bottom"
            class="fa fa-exclamation-triangle fa-fw"
            title="Invalid Input"
          ></i>
          <span class="text-key pl-1">{{ item[textKey] }}</span>
          <div class="has-clearable d-inline pl-4">
            <button type="button" class="close show p-0" style="color: #000">
              <span aria-hidden="true">
                <i class="fa fa-times-circle"></i>
              </span>
            </button>
          </div>
        </span>
        <span v-if="items.length > limit" @click="showMore" class="badge badge-subtle badge-dark">{{items.length - limit}} more ..</span>
        <span v-if="items.length === limit && items.length !== 8" @click="showLess" class="badge badge-subtle badge-dark">Less</span>
      </div>
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
  data() {
    return {
      limit: 8,
    }
  },
  methods: {
    removeItem(itemIndex: number) {
      this.items.splice(itemIndex, 1);
    },
    showMore() {
      this.limit = this.items.length
    },
    showLess() {
      this.limit = 8;
    }
  }
});
</script>
<style>
.publisher-actions {
    height: auto;
    max-height: 155px;
    overflow-x: auto;
    overflow-y: auto;
}
</style>