<template>
  <div id="app" class="mh-100 h-100">
    <div class="nav-scroller border-bottom">
      <!-- .nav-tabs -->
      <ul class="nav nav-tabs">
        <li v-for="(page, index) in pages" :key="index" class="nav-item">
          <router-link
            v-slot="{ href, route, navigate, isActive, isExactActive }"
            :to="{path:page.router}"
          >
            <a class="nav-link" @click="navigate" :active="isActive" :class="[isExactActive && 'active']" data-toggle="tab" :href="href" >{{  page.name  }}</a>
          </router-link>
        </li>
      </ul>
      <!-- /.nav-tabs -->
    </div>

    <div class="container mh-100 h-100">
      <div class="row justify-content-center mh-100 h-100">
        <div class="col-lg-6 col-md-8 col-s-12 col-xs-12">
          <router-view></router-view>
        </div>
      </div>
      <div class="row justify-content-center">
        <div class="col-lg-6 col-md-8 col-s-12 col-xs-12 text-center"></div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  data() {
    return {
      pages: [
        {
          name: "Home",
          router: "/"
        },
        {
          name: "Skeleton",
          router: "/skeleton"
        }
      ]
    };
  },
  components: {}
});
</script>


<style>
@import url("https://uselooper.com/assets/stylesheets/theme.min.css");
@import url("https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css");
</style>
