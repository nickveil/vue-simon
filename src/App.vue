<template>

  <div id="simon">
    <h1 class=text-center>Vue-Simon</h1>

    <div class="container text-center">
      <div class="row text-center">
        <div class='col-xl-2'>
          <button class="btn btn-outline-primary btn-lg">1</button>
          <button class="btn btn-outline-success btn-lg">1</button>
        </div>
      </div>
      <div class="row text-center">
        <div class='col-xl-2'>
          <button class="btn btn-outline-warning btn-lg">1</button>
          <button class="btn btn-outline-danger btn-lg">1</button>
        </div>
      </div>
    </div>
    <div class= "col-xs-12 text-center">
      <button class="btn btn-dark btn-sm">Start</button>
    </div>
  </div>

</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
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
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.btn{
  height: 150px;
  width: 150px;
  margin: 20px;
}
.btn-sm{
  padding-bottom: 15px;
  height: 35px;
  width: 75px;
  align-items: center;
}

</style>
