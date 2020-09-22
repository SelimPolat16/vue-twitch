<template>
  <div id="topNavbarLinksContainer">
     <router-link to="/">Get Top Games</router-link> |
     <router-link to="/GetTopStreamers">Get Top Streamers</router-link>
  </div>
</template>

<script>
export default {
    name: 'topNavbarLinks'
}
</script>

<style>
#topNavbarLinksContainer{
    margin-bottom: 10px;
}
#topNavbarLinksContainer a {
    font-size: 25px;
    padding: 10px;
}
</style>