<template>
  <div ref="taggingInput" class="tag-input-component has-typeahead-scrollable">
    <div class="field-wrapper" @click="foucsField">
      <span>
        <input
          ref="tagInputField"
          class="tag-input-field"
          v-model="inputItem"
          type="text"
          placeholder="Type your color name"
          @input="fetchSuggestion"
          tabindex="0"
          @keydown="test"
        />
        <!-- @focus="fetchSuggestion" -->
        <!--  -->
      </span>
      <div class="badge-wrapper" @click="foucsField">
        <span
          v-for="(item, index) in items"
          :key="index"
          class="badge badge-pill badge-subtle"
          :class="{ 'badge-success': !item.isNew,
                  'badge-warning': item.isNew,
                  'badge-danger': item.isInvalid}"
        >
          <span class="text-key">{{ item[textKey] }}</span>
          <a class="close" @click="removeItem(index)">x</a>
        </span>
      </div>
    </div>
    <div
      class="tt-menu tt-open"
      style="position: absolute; top: 100%; left: 0px; z-index: 100; display: block"
      ref="suggestedListWrapper"
      v-show="recivedItems.length || showSpinner"
    >
      <Spinner style="padding: .5rem .75rem; margin: .5rem 0 0;" :can-show="showSpinner" />
      <div class="tt-dataset">
        <div
          class="list-group-bordered"
          :style="[recivedItems.length ? {display: 'block'} : {display: 'none'}]"
        >
          <div
            v-for="(item, index) in recivedItems"
            :key="index"
            class="list-group-item list-group-item-action"
            @click="addItem(index)"
            :tabindex="index + 1"
            ref="listItems"
            @keydown="test"
          >
            <div v-if="!item.isNew" class="list-group-item-figure">
              <div
                class="tile tile-circle"
                :class="getCircleColor(item[textKey].length)"
              >{{ item[textKey][0].toUpperCase() }}</div>
            </div>
            <div
              class="list-group-item-body"
            >{{ item.isNew ? '"' + item[textKey] + '"' : item[textKey]}}</div>
            <button
              v-if="item.isNew"
              type="button"
              class="btn btn-icon bg-primary mr-1"
              style="color: #fff"
              data-toggle="tooltip"
              title
              data-original-title="Private message"
            >
              <i class="fa fa-plus"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue, { PropType } from "vue";

import Spinner from "./Spinner.vue";

export default Vue.extend({
  props: {
    fetch: {
      type: Function as PropType<
        (textKey: string, query: string) => Promise<Array<{}>>
      >,
      required: true
    },
    textKey: {
      type: String,
      required: true
    },
    idKey: {
      type: [String, Number],
      required: true
    }
  },
  components: {
    Spinner
  },
  data() {
    const items: Array<{}> = [];
    const recivedItems: Array<{}> = [];
    const inputItem = "";
    let typingTimer!: number;
    const bgColorList: Array<string> = [
      "bg-yellow",
      "bg-purple",
      "bg-red",
      "bg-blue",
      "bg-green",
      "bg-orange",
      "bg-gray",
      "gray-dark"
    ];

    return {
      inputItem,
      items,
      recivedItems,
      typingTimer,
      bgColorList,
      showSpinner: false,
      delayCounter: 0,
      focus: 0
    };
  },
  methods: {
    addItem(itemIndex: number) {
      if (this.isDuplicated(itemIndex)) {
        this.recivedItems.splice(itemIndex, 1);
        return;
      }

      this.items.push(this.recivedItems[itemIndex]);
      this.recivedItems.splice(itemIndex, 1);
      this.recivedItems = [];
      this.inputItem = "";
      return;
    },
    addNewSuggestion() {
      if (!this.inputItem) {
        return;
      }

      this.recivedItems.push({
        [this.textKey]: this.inputItem,
        isNew: true,
        [this.idKey]: this.getNewItemID()
      });
    },
    isNewItem() {
      const filterItems = (item: {}, index: number, array: {}[]) => {
        const tempItem = Object.create(item);
        return tempItem[this.textKey] === this.inputItem;
      };

      return !this.recivedItems.filter(filterItems).length;
    },
    isDuplicated(itemIndex: number) {
      const recivedItem = Object.create(this.recivedItems[itemIndex]);
      const filterItems = (item: {}, index: number, array: {}[]) => {
        const tempItem = Object.create(item);
        return tempItem[this.textKey] === recivedItem[this.textKey];
      };

      return !!this.items.filter(filterItems).length;
    },
    getNewItemID() {
      const filterItems = (item: {}, index: number, array: {}[]) => {
        const tempItem = Object.create(item);
        return tempItem.isNew;
      };
      const lengthOfNewItems = this.items.filter(filterItems).length;
      const newItemID = !lengthOfNewItems ? -1 : lengthOfNewItems * -1 - 1;
      return newItemID;
    },
    foucsField() {
      const el = this.$refs.tagInputField as HTMLInputElement;
      el.focus();
    },
    fetchSuggestion() {
      if (this.inputItem.length < 2) {
        this.recivedItems = [];
        return;
      }
      clearTimeout(this.typingTimer);

      this.delayCounter++;
      const requestOrder = this.delayCounter;

      this.recivedItems = [];
      const query = this.inputItem as string;
      this.typingTimer = setTimeout(() => {
        this.showSpinner = true;
        this.fetch(this.textKey, query)
          .then(res => {
            console.log("req = > ", this.delayCounter, requestOrder);
            if (this.delayCounter !== requestOrder) {
              return;
            }
            this.recivedItems = res;
            if (!this.recivedItems.length) {
              this.addNewSuggestion();
            }
          })
          .catch(() => {
            this.recivedItems = [];
            this.showSpinner = false;
          })
          .finally(() => {
            console.log(this.delayCounter, requestOrder);
            if (this.delayCounter !== requestOrder) {
              return;
            }
            console.log("closed");
            this.showSpinner = false;
          });
      }, 500);
    },
    getCircleColor(nameLength: number) {
      return this.bgColorList[
        Math.abs(nameLength as number) % this.bgColorList.length
      ];
    },
    closeSuggestionList(event: Event) {
      console.log("closeSuggestionList");
      this.recivedItems = [];
      // this.showSpinner = false;
    },
    removeItem(itemIndex: number) {
      this.items.splice(itemIndex, 1);
    },
    test: function(event: KeyboardEvent) {
      if ([38, 40].indexOf(event.keyCode) === -1) {
        return;
      }
      switch (event.keyCode) {
        case 38:
          if (this.focus > 0) {
            this.focus--;
            if (!this.focus) {
              this.foucsField();
              return;
            }
          }
          break;
        case 40:
          if (!this.focus) {
            this.focus = 1;
          } else if (this.focus < this.recivedItems.length) {
            this.focus++;
          }
          break;
      }
      setTimeout(() => {
        const listItems = this.$refs.listItems as Array<HTMLDivElement>;
        listItems[this.focus - 1].focus();
      }, 100);
    },
    handleFocusOut(event: Event) {
      const taggingInput = this.$refs.taggingInput as HTMLElement;
      const target = event.target as HTMLElement;
      if (target.closest(".tag-input-component") !== null) {
        console.log("open");
      } else {
        this.recivedItems = [];
        this.focus = 0;
      }
    }
  },
  created() {
    document.body.addEventListener("click", this.handleFocusOut);
  },
  destroyed() {
    document.body.removeEventListener("click", this.handleFocusOut);
  }
});
</script>

<style scoped>
.badge.badge-pill.badge-subtle {
  display: inline-block;
  cursor: move;
  margin: 5px;
}

.field-wrapper {
  padding: 5px;
  min-height: 50px;
  width: 100%;
  max-width: 90rem;
  flex-direction: row;
  background: #fff;
  border: solid 1px #cfcfcf;
}

input.tag-input-field {
  width: 100%;
  height: 50px;
}

.badge-wrapper {
  /* padding: 5px; */

  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-wrap: wrap;
}

.tag-input-field {
  padding-left: 10px;
  border: none;
  outline: none;
  box-sizing: border-box;
}

.badge .text-key {
  padding-left: 5px;
}

.badge .close {
  float: none;
  font-size: inherit;
  display: inline-block;
  background-color: #19191945;
  color: #14141f60;
  box-sizing: border-box;
  border-radius: 100%;
  font-weight: bolder;
  cursor: pointer;
  margin-left: 5px;
  padding-bottom: 3px;
  margin-top: 2px;
  width: 15px;
}

.badge:hover a.close {
  background-color: #80808026;
  color: black;
}

.autocomplete-wrapper {
  width: 300px;
  overflow: hidden;
}

.list-group-item:last-child {
  border-width: 0;
}

div.focus {
  border: 1px solid blue !important;
}
</style>
