<template>
  <div
    class="publisher-actions form-control"
    :class="{ 'border border-primary': isFocused}"
    :style="[ isFocused === true ? {'boxShadow': '0 0 0 1px #346cb0'} : '']"
  >
    <div class="publisher-tools mr-auto">
      <ul class="nav justify-content-start">
        <li clss="nav-item" v-for="(item, index) in items" :key="index">
          <span
            class="badge badge-pill border mr-1"
            :class="{ 'badge-success': !item.isNew,
                    'badge-warning ': item.isNew,
                    'badge-danger': !item.isValid}"
            v-show="isFocused || index < limit"
            @dblclick="expandGroup(index)"
          >
            <i
              v-if="item.isValid && !item.isNew && !item.group"
              title="Correct"
              class="fa fa-check-circle fa-fw"
            ></i>
            <i
              v-if="item.isValid && !item.isNew && item.group"
              title="Double click to expand"
              class="fa fa-users fa-fw"
            ></i>
            <i
              v-else-if="item.isValid && item.isNew"
              title="New Item"
              class="fa fa-exclamation-circle fa-fw"
            ></i>
            <i
              v-else-if="!item.isValid"
              title="Invalid Input"
              class="fa fa-exclamation-triangle fa-fw"
            ></i>
            <span class="text-key pl-1">{{ item[textKey] }}</span>
            <div class="has-clearable d-inline pl-4">
              <button
                type="button"
                class="close show p-0"
                style="color: #000"
                @click="removeItem(index)"
              >
                <span aria-hidden="true">
                  <i class="fa fa-times-circle"></i>
                </span>
              </button>
            </div>
          </span>
        </li>
        <li clss="nav-item">
          <span
            v-if="!isFocused && items.length > limit"
            @click="showMore"
            class="badge badge-subtle badge-dark"
          >{{items.length - limit}} more ..</span>
        </li>
        <li clss="nav-item">
          <slot></slot>
        </li>
      </ul>
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
    },
    isFocused: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      limit: 8
    };
  },
  methods: {
    removeItem(itemIndex: number) {
      this.items.splice(itemIndex, 1);
    },
    showMore() {
      this.limit = this.items.length;
    },
    expandGroup(index: number ) {
      const item: any = this.items[index]
      if(!item.group) {
        return
      }
      this.items.splice(index, 1, ...item.group);
    }
  }
});
</script>
<style>
.publisher-actions {
  height: auto;
  max-height: 155px;
  overflow-y: auto;
}
</style>