<template>
  <div id="app">
      <b-container fluid>
          <b-row class="form-group">
              <b-col></b-col>
              <b-col>
                  <h3>CSV To JSON Converter</h3>
              </b-col>
              <b-col></b-col>
          </b-row>
          <b-row>
              <b-col></b-col>
              <b-col class="card-style">
                  <Converter/>
              </b-col>
              <b-col></b-col>
          </b-row>
      </b-container>
  </div>
</template>

<script>
import Converter from './components/converter.vue'

export default {
  name: 'App',
  components: {
    Converter
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.card-style {
    background-color: rgba(0,0,0,.03);
    padding: 15px;
    border: 1px solid #ebebeb;
    border-radius: 10px;
}
</style>
