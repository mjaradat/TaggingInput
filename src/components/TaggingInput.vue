<template>
  <div
    ref="taggingInput"
    :data-component-id="compontentID"
    class="form-group tag-input-component has-typeahead-scrollable"
    @click="_foucsField"
  >
    <div class="media">
      <div class="media-body mw-100">
        <label for="tagInputField">{{ fieldLable }}</label>
        <div ref="publisher" class="publisher position-relative keep-focus focus" :class="{active: !focus}">
          <Badges :items="items" text-key="email" :isFocused="canShow" @click="_foucsField">
            <span class="publisher-input position-relative m-0">
              <input
                id="tagInputField"
                class="form-control border-0 d-block mw-100 p-0"
                type="text"
                tabindex="0"
                autocomplete="off"
                style="outline: none; box-shadow: none; padding: 0px; outline: 0px !important;"
                ref="tagInputField"
                v-model="inputItem"
                :placeholder="[this.items.length? '' : (placeholder || 'Write Somthing')]"
                :style="{'width': inputWidth}"
                @keydown="_selectItemByArrowKeys"
                @keydown.enter="_addItem(focus - 1); _setInputWidth()"
                @focus="(event) => {_toggleListItem(event, true); _setInputWidth()}"
                @input="_fetchSuggestion(); _setInputWidth()"
              />
            </span>
          </Badges>
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
                  class="list-group-item list-group-item-action"
                  v-for="(item, index) in recivedItems"
                  :key="index"
                  :tabindex="index + 1"
                  ref="listItems"
                  @click="_addItem(index); _setInputWidth()"
                  @keydown="_selectItemByArrowKeys"
                  @mouseover="_selectItemOnMousover(index)"
                  :class="[(index + 1) === focus? 'bg-primary text-white': '']"
                  :style="[index === recivedItems.length - 1? {'border': 0} : '']"
                >
                  <div v-if="!item.isNew" class="list-group-item-figure">
                    <div
                      class="tile tile-circle"
                      :class="_getCircleColor(item[textKey].length)"
                    >{{ item[textKey][0].toUpperCase() }}</div>
                  </div>
                  <div
                    class="list-group-item-body"
                    v-if="!item.isNew"
                    v-html="_setHighlightedText(item[textKey])"
                  ></div>
                  <div
                    v-else-if="item.isNew"
                    class="list-group-item-body"
                  >{{ '"' + item[textKey] + '"' }}</div>
                  <small class="list-group-item-body text-muted text-right"
                         :class="[(index + 1) === focus? 'text-white': '']"
                         :style="{fontSize: '0.5rem'}">
                    <i class="fa fa-exclamation-circle"></i>
                    click or press enter to add
                  </small>
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
import Badges from "./Badges.vue";

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
    fieldLable: {
      type: String,
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
    Badges
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
      inputWidth: "",
      compontentID: new Date().getTime()
    };
  },
  methods: {
    _addItem(itemIndex: number) {
      if (
        (!itemIndex && !this.recivedItems.length) ||
        !this.inputItem ||
        this.showSpinner ||
        this._isDuplicated(itemIndex)
      ) {
        return;
      }
      const item: any = this.recivedItems[itemIndex];
      this.items.push(item);

      if (!this.isMultiSelect || item.isNew) {
        this.recivedItems = [];
        this.inputItem = "";
        return;
      }

      if (!this.recivedItems.length) {
        this.inputItem = "";
      }
      return;
    },
    _addNewSuggestion() {
      if (!this.inputItem) {
        return;
      }

      this.recivedItems.push({
        [this.textKey]: this.inputItem,
        isNew: true,
        [this.idKey]: this._getNewItemID(),
        isValid: true
      });
    },
    _isDuplicated(itemIndex: number) {
      if (!this.recivedItems.length) {
        return false;
      }
      const recivedItem = Object.create(this.recivedItems[itemIndex]);
      const filterItems = (item: {}, index: number, array: {}[]) => {
        const tempItem = Object.create(item);
        return tempItem[this.textKey] === recivedItem[this.textKey];
      };

      return !!this.items.filter(filterItems).length;
    },
    _getNewItemID() {
      if (!this.items.length) {
        return -1;
      }

      const filterItems = (item: {}, index: number, array: {}[]) => {
        const tempItem = Object.create(item);
        return tempItem.isNew;
      };

      const lengthOfNewItems = this.items.filter(filterItems).length;
      const newItemID = !lengthOfNewItems ? -1 : lengthOfNewItems * -1 - 1;
      return newItemID;
    },
    _foucsField() {
      const el = this.$refs.tagInputField as HTMLInputElement;
      el.focus();
      setTimeout(() => {
        el.scrollIntoView({
          block: "center",
          behavior: "smooth"
        });
      }, 300);
    },
    _fetchSuggestion() {
      //To ignore the last input event that triggered
      //after clearing the content of the input field totaly by backspace
      if (this.inputItem.length < 2) {
        this.recivedItems = [];
        this.showSpinner = false;
        return;
      }

      //To clear the previous timeout
      //On input event
      clearTimeout(this.typingTimer);

      //Used to show that last
      //requested list items
      this.delayCounter++;
      const requestOrder = this.delayCounter;

      this.recivedItems = [];
      const query = this.inputItem as string;
      this.typingTimer = setTimeout(() => {
        this.showSpinner = true;
        this.fetch(query, this.textKey)
          .then(res => {
            //if this condi. is true, this means there is
            //upcoming another request, so no need to show this request.
            if (
              this.delayCounter !== requestOrder ||
              this.inputItem.length < 2
            ) {
              return;
            }

            this.recivedItems = res;
            if (!this.recivedItems.length) {
              this._addNewSuggestion();
            }
          })
          .catch(() => {
            this.recivedItems = [];
            this.showSpinner = false;
          })
          .finally(() => {
            if (this.delayCounter !== requestOrder) {
              return;
            }
            this.showSpinner = false;
          });
      }, 500);
    },
    _getCircleColor(nameLength: number) {
      return this.bgColorList[
        Math.abs(nameLength as number) % this.bgColorList.length
      ];
    },
    _selectItemByIndex(index: number, direction: number) {
      const listItems = this.$refs.listItems as Array<HTMLDivElement>;
      if (!listItems || !listItems.length) {
        return;
      }
      //listItems[index - direction].style.backgroundColor = "";
      //listItems[index].style.backgroundColor = "#346cb0";
      this.focus = index + 1;
      listItems[index].scrollIntoView({
        block: "center",
        behavior: (this.focus === 1 || (this.focus === this.recivedItems.length) ? "auto" :"smooth" )
      });
    },
    _selectItemOnMousover(index: number){
      const listItems = this.$refs.listItems as Array<HTMLDivElement>;
      if (!listItems || !listItems.length) {
        return;
      }

      //listItems[this.focus - 1].style.backgroundColor = "";
      //listItems[index].style.backgroundColor = "#f6f7f9";
      this.focus = index + 1;
    },
    _selectItemByArrowKeys(event: KeyboardEvent) {
      if ([38, 40].indexOf(event.keyCode) === -1) {
        return;
      }
      let direction = -1; //To Top
      switch (event.keyCode) {
        case 38:
          if (!this.focus) {
            this.focus = 1;
          } else if (this.focus > 1) {
            this.focus--;
          } else if (this.focus === 1) {
            this.focus = this.recivedItems.length;
          }
          direction = -1
          break;
        case 40:
          if (!this.focus) {
            this.focus = 1;
          } else if (this.focus < this.recivedItems.length) {
            this.focus++;
          } else if (this.focus === this.recivedItems.length) {
            this.focus = 1;
          }
          direction = 1
          break;
      }
      setTimeout(() => {
        this._selectItemByIndex(this.focus - 1, direction);
      }, 100);
    },
    _toggleListItem(event: Event, canShow: boolean) {
      this.canShow = canShow;
      document.removeEventListener("click", this._handleFocusOut, true);
      document.addEventListener("click", this._handleFocusOut, true);
    },
    _handleFocusOut(event: Event) {
      const taggingInput = this.$refs.taggingInput as HTMLElement;
      const target = event.target as HTMLElement;
      const taggingInputComponent = target.closest(
        ".tag-input-component[data-component-id='" + this.compontentID + "']"
      );
      if (taggingInputComponent !== null) {
        console.log("open");
      } else {
        document.removeEventListener("click", this._handleFocusOut, true);
        this.canShow = false;
        this.focus = 1;
        this.inputItem = "";
        this.recivedItems = [];
        this._setInputWidth();
      }
    },
    _setInputWidth() {
      const inputWidth = this.inputItem.length * 0.75;
      if (!inputWidth && !this.items.length && this.placeholder.length) {
        this.inputWidth = this.placeholder.length + "rem";
        return;
      }

      if (inputWidth) {
        this.inputWidth = inputWidth + 0.75 + "rem";
      } else {
        this.inputWidth = "0.75rem";
      }
    },
    _setHighlightedText(text: string) {
      this.inputItem;
      return text.replace(
        new RegExp(this.inputItem + "(?!([^<]+)?<)", "gi"),
        "<b>$&</b>"
      );
    },
    getSelected() {
      return this.items;
    }
  },
  created() {
    this._setInputWidth();
  }
});
</script>

