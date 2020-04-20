<template>
  <div ref="taggingInput" :data-component-id="compontentID" class="tag-input-component has-typeahead-scrollable" @click="foucsField">
    <div class="card">
      <!-- .media-body -->
      <div class="card-body">
        <div class="media">
          <!-- .media-body -->
          <div class="media-body">
            <!-- .publisher -->
            <div ref="publisher" class="publisher keep-focus focus" :class="{active: !focus}">
              <!-- Bage Containers -->
              <Tags :items="items" :text-key="textKey" @click="foucsField" />

              <div class="publisher-input position-relative">
                <input
                  id="publisherInput1"
                  class="form-control"
                  :placeholder="placeholder || 'Write Somthing'"
                  ref="tagInputField"
                  v-model="inputItem"
                  type="text"
                  @input="fetchSuggestion"
                  tabindex="0"
                  @keydown="selectItemByArrowKeys"
                  @keydown.enter="addItem(focus - 1)"
                  @focus="(event) => {toggleListItem(event, true)}"
                />
                <div
                  class="tt-menu tt-open"
                  style="position: absolute; top: 100%; left: 0px; z-index: 100; display: block"
                  ref="suggestedListWrapper"
                  v-if="(canShow && recivedItems.length) || showSpinner"
                >
                  <Spinner class="p-1 mt-1" :can-show="showSpinner" />
                  <div class="tt-dataset">
                    <div class="list-group-bordered" v-show="recivedItems">
                      <div
                        v-for="(item, index) in recivedItems"
                        :key="index"
                        class="list-group-item list-group-item-action"
                        @click="addItem(index)"
                        :tabindex="index + 1"
                        ref="listItems"
                        @keydown="selectItemByArrowKeys"
                        :class="[(index + 1) === focus? '': '']"
                        @mouseover="selectItemByIndex(index)"
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
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue, { PropType } from "vue";

import Spinner from "./Spinner.vue";
import Tags from "./Tags.vue";

export default Vue.extend({
  props: {
    fetch: {
      type: Function as PropType<
        (query: string, textKey: string) => Promise<Array<{}>>
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
    },
    placeholder: {
      type: String
    },
    isMultiSelect: {
      type: Boolean,
      default: false
    }
  },
  components: {
    Spinner,
    Tags
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
      focus: 1,
      canShow: false,
      compontentID: new Date().getTime()
    };
  },
  methods: {
    addItem(itemIndex: number) {
      if (this.isDuplicated(itemIndex)) {
        // this.recivedItems.splice(itemIndex, 1);
        return;
      }

      this.items.push(this.recivedItems[itemIndex]);
      // this.recivedItems.splice(itemIndex, 1);
      if (!this.isMultiSelect) {
        this.recivedItems = [];
        this.inputItem = "";
      }

      if (!this.recivedItems.length) {
        this.inputItem = "";
      }
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
        this.fetch(query, this.textKey)
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
    selectItemByIndex(index: number) {
      const listItems = this.$refs.listItems as Array<HTMLDivElement>;
      if (!listItems || !listItems.length) {
        return;
      }
      this.focus = index + 1;
      listItems[index].scrollIntoView({
        block: "center",
        behavior: "smooth"
      });
    },
    selectItemByArrowKeys(event: KeyboardEvent) {
      if ([38, 40].indexOf(event.keyCode) === -1) {
        return;
      }
      switch (event.keyCode) {
        case 38:
          if (!this.focus) {
            this.focus = 1;
          } else if (this.focus > 1) {
            this.focus--;
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
        this.selectItemByIndex(this.focus - 1);
      }, 100);
    },
    toggleListItem(event: Event, canShow: boolean) {
      
      this.canShow = canShow;
      console.log("event added");
      document.removeEventListener("click", this.handleFocusOut, true);
      document.addEventListener("click", this.handleFocusOut, true);

      //console.log(document.activeElement)
      //this.canShow = canShow;
      // this.toggleListItemTimer = setTimeout(() => {
      //   this.canShow = canShow;
      //   this.focus = 1;
      // }, 200)

      // const taggingInput = this.$refs.taggingInput as HTMLElement;
      // const target = event.target as HTMLElement;
      // console.log(target)
      // if (target.closest(".tag-input-component") !== null) {
      //   this.canShow = true
      // } else {
      //   console.log("close")
      //   this.canShow = false;
      // }
    },
    handleFocusOut(event: Event) {
      const taggingInput = this.$refs.taggingInput as HTMLElement;
      const target = event.target as HTMLElement;
      console.log(target.closest(".tag-input-component[data-component-id='"+ this.compontentID +"']"));
      if (target.closest(".tag-input-component[data-component-id='"+ this.compontentID +"']") !== null) {
        console.log("open");
      } else {
        //this.recivedItems = [];
        //this.focus = 0;
        document.removeEventListener("click", this.handleFocusOut, true);
        console.log("close");
        this.canShow = false;
        this.focus = 1;
      }
    },
  },
  created() {
    //document.addEventListener("click", this.handleFocusOut, true);
  },
  destroyed() {
    //document.removeEventListener("click", this.handleFocusOut, true);
  }
});
</script>

