<template>
  <div>
    <h1>Validator</h1>

    <form @submit.prevent="validate">
      <input v-model="targetUrl" type="url" required/>
      <button>Validate</button>
    </form>

    <div v-if="data">
      <pre>{{data}}</pre>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import axios from 'axios';

const http = axios.create({
  headers: {
    'Accept': 'text/turtle'
  }
});

@Component
export default class Validator extends Vue {
  private targetUrl = '';
  private data = null;

  validate() {
    const response = http.get(`http://localhost:8080/validate?resource=${this.targetUrl}`);

    response.then((response) => { this.data = response.data }, (reason) => this.data = reason);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
pre {
  margin: 2em auto;
  width: 75%;
  text-align: left;
}

input {
  width: 40%;
}
</style>
