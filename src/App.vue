<template>
  <div class="app" id="app">
    <div class="col text-center">
      <img alt="Vue logo" src="./assets/logo.png" />
    </div>
    <div class="contaier">
      <div class="row justify-content-center">
        <div class="col-8 text-center">
          <TaggingInput ref="tagging" :fetch="getData" text-key="color" id-key="value" />
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import TaggingInput from "./components/TaggingInput.vue";

export default Vue.extend({
  methods: {
    getData(textKey: string, query: string): Promise<Array<any>> {
      return new Promise<Array<any>>((resolve, reject) => {
        const items = [
          {
            color: "red",
            value: "#f00"
          },
          {
            color: "green",
            value: "#0f0"
          },
          {
            color: "blue",
            value: "#00f"
          },
          {
            color: "cyan",
            value: "#0ff"
          },
          {
            color: "magenta",
            value: "#f0f"
          },
          {
            color: "yellow",
            value: "#ff0"
          },
          {
            color: "black",
            value: "#000"
          }
        ];

        setTimeout(function() {
          if (!query) {
            reject();
          }
          if (query === "@showall") {
            resolve(items);
            return;
          }
          const filteredItems = items.filter(item => {
            const tempItem = Object.create(item);
            return tempItem[textKey].startsWith(query);
          });
          resolve(filteredItems);
        }, 1500);
      });
    }
  },
  components: {
    TaggingInput,
  },
  mounted() {
    console.log(this.$refs.tagging);
  }
});
</script>

<style>
@import url("https://uselooper.com/assets/stylesheets/theme.min.css");
@import url("https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css");

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
