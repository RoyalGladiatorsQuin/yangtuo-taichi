<template>
  <Entry></Entry>
</template>

<script>
import Entry from './components/Entry.vue'
export default {
  name: 'App',
  components: {
    Entry,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

body{
  margin: 0;
}
html{
  color: #000;
  overflow-x: hidden;

  overscroll-behavior-y: none;
  font-size: 10px;
  height: 100%;
}

</style>
