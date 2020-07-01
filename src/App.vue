<template>
  <div id="app" class="container-fluid">
    <div id="nav" class="">
        <router-link to="/">
          <img src="../public/usething/icon/round-schedule-24px.svg" />
        </router-link>
        <router-link to="/list">
        <img src="../public/usething/icon/round-list-24px.svg">
        </router-link>
        <router-link to="/settings">
        <img src="../public/usething/icon/round-library_music-24px.svg">
        </router-link>
    </div>
    <keep-alive>
      <router-view />
    </keep-alive>
  </div>
</template>

<script>
export default {
  // data () {
  // }
}
</script>

<style lang="stylus" scoped>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@900&display=swap')
</style>
