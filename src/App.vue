<template>
  <div id="app">


    <b-container>
          <Naslovnica naslov="Projektni zadatak iz kolegija Alogritmi u primjeni: IMDb clone App" podnaslov="Student: Mirko Miličić, II. godina informatike, Orašje" />

      <PretragaFilmova />
      
    </b-container>

  </div>
</template>

<script>
  import Naslovnica from './components/Naslovnica'
  import PretragaFilmova from './components/PretragaFilmova'
 

  export default {
    name: 'App',
    components: {
      Naslovnica,
      PretragaFilmova,
      

    }
  }
</script>

<style>

</style>