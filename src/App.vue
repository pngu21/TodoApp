<template>
  <div id="app" class="bg-image">
    <!-- <img alt="Vue logo" src="./assets/logo.png" class="logo"> -->
    <Todo/>
  </div>
</template>

<script>
import Todo from './components/Todo'

export default {
  name: 'App',
  components: {
    Todo,
  }
}
</script>

<style>

.bg-image{
  background-image: url("https://desktopwalls.net/wp-content/uploads/2016/12/Polygonal%20Reindeer%20Colorful%20Desktop%20Wallpaper.jpg");
  height: 100vh;
  background-size: cover;

}
</style>
