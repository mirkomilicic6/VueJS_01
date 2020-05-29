<template>
  <div>
    <b-container>
      <b-jumbotron class="jumbotron" :header="naslov" :lead="podnaslov">
        <b-button class="button" variant="primary" href="http://www.omdbapi.com/" target="blank">API izvor</b-button>
        <b-button class="button" variant="secondary" href="https://github.com/mirkomilicic6/vueJS_FilmoviAPI" target="blank">GitHub link</b-button>
      </b-jumbotron>
    </b-container>
  </div>
</template>

<script>
  export default {
    name: "Naslovnica",
    props: ['naslov', 'podnaslov']
  };
</script>

<style>
.jumbotron {
  margin-top: 1rem;
}
.button {
  margin: 2px;
}
</style>