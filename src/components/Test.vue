<template>
  <div>
    <input ref="input" type="text" @keydown="test" tabindex="0">
    <p>fkjsdfhklsjergklrntdgh</p>
    <div ref="test" v-for="(item, index) in items" :key="index" :class="{ focus: index === focus }">
       <div>{{ item.title }}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        { title: "First" },
        { title: "Second" },
        { title: "Third" }
      ],
      focus: null
    };
  },
  methods: {
    test: function(event) {
      switch (event.keyCode) {
        case 38:
          if (this.focus === null) {
            this.focus = 0;
          } else if (this.focus > 0) {
            this.focus--;
          }
          break;
        case 40:
          if (this.focus === null) {
            this.focus = 0;
          } else if (this.focus < this.items.length - 1) {
            this.focus++;
          }
          break;
      }
    }
  }
};
</script>

<style>
div.focus {
  border: 1px solid blue;
}
</style>