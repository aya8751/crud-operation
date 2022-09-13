
<template>
  <div class="container">
    <h3 class="d-flex justify-content-center my-5">
      CRUD Application
    </h3>
    <CrudList />
  </div>
</template>

<script>
import CrudList from './components/CrudList.vue'
export default {
  data() {
    return { 
    }
  },
  components: {
    CrudList
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
@import'~bootstrap/dist/css/bootstrap.css'
</style>
