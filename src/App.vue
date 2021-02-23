<template>
  <div id="app">
    <p id="nav">
      <router-link to="/">tabMode</router-link>
      <router-link to="/about">searchMode</router-link>
    </p>
    <app-header></app-header>
    <router-view/>
    <app-footer></app-footer>
  </div>
</template>
<script>
import AppHeader from "@/components/AppHeader";
import AppFooter from "@/components/AppFooter";
export default{
  components :  {
    AppHeader, AppFooter
  }
}
</script>
<style lang="scss">
  @import "assets/scss/style";
</style>
