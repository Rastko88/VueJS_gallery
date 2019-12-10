<template>
  <div id="app">
    <HeaderComponent />
    <ImagesListComponent />
  </div>
</template>

<script>
  import HeaderComponent from './components/HeaderComponent.vue';
  import ImagesListComponent from './components/ImagesListComponent.vue';

  export default {
    name: 'app',
    components: {
      HeaderComponent,
      ImagesListComponent
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    background-color: #CBE0D7;
    padding: 0;
    width: 100%;
    height: 100%;
    margin: 0 auto;
  }
</style>
