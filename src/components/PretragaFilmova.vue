<template>
  <b-container>
    <div>
      <!--unos i pretraga-->
      <b-row>
        <b-col cols="4">
          <b-form-input v-model="searchText" @keyup.enter="dohvatiPodatke" placeholder="Unesi naziv filma..">
          </b-form-input>
        </b-col>

        <b-col>
          <b-button @click="dohvatiPodatke">Pretraži</b-button>
        </b-col>
      </b-row>
      <hr>
      <br>
      <!--kraj unosa i pretrage-->

      <!--ispis pronađenog-->
      <b-row>
        <b-col v-if="PronadeniFilmovi.Response === 'True'">
          <p><b>Pronađeni film po nazivu:</b></p>
          <div class="pronadeno">
            <div class="mt-4">
              <h3> {{ PronadeniFilmovi.Title + ' (' + PronadeniFilmovi.Year + ')' }} </h3>
              <b-card :img-src="PronadeniFilmovi.Poster" :img-alt="PronadeniFilmovi.Title" img-left class="mb-3">
                <b-card-text>
                  <p><b>Žanr:</b> {{ PronadeniFilmovi.Genre }}</p>
                  <p><b>Zaplet:</b> {{ PronadeniFilmovi.Plot }}</p>
                  <p><b>Glumci:</b> {{ PronadeniFilmovi.Actors }}</p>
                  <p><b>Redatelj:</b> {{ PronadeniFilmovi.Director }}</p>
                  <p><b>Trajanje:</b> {{ PronadeniFilmovi.Runtime }}</p>
                  <p><b>IMDb ocjena:</b>
                    {{ PronadeniFilmovi.imdbRating + ' na ' + PronadeniFilmovi.imdbVotes + ' glasova.' }}</p>
                  <b-button class="button" :href="'https://www.imdb.com/title/' + PronadeniFilmovi.imdbID"
                    target="_blank" variant="warning"><b> Imdb link </b> </b-button>
                  <b-button class="button"
                    :href="'https://www.youtube.com/results?search_query=' + PronadeniFilmovi.Title + ' trailer'"
                    target="_blank" variant="danger"><b> YouTube traileri </b> </b-button>
                </b-card-text>
              </b-card>
            </div>
          </div>
        </b-col>
      </b-row>
      <hr>
      <!--kraj ispisa pronađenog-->

      <!--pocetak notFound-->
      <b-row class="notFound">
        <b-col v-if="PronadeniFilmovi.Response === 'False'">
          <b-alert show variant="primary">Film nije pronađen!</b-alert>
        </b-col>
      </b-row>
      <!--kraj notFound-->

      <!--pocetak sličnih-->
      <b-col v-if="PronadeniFilmovi.Response=== 'True'">
        <p><b>Filmovi sličnog naziva:</b></p>

      </b-col>
      <b-row>

        <b-modal size="lg" id="modal-center" centered :title="imdbSlicnog.Title">

          <div>
            <b-card :img-src="imdbSlicnog.Poster" :img-alt="imdbSlicnog.Title" img-left class="mb-3">
              <b-card-text>
                <p><b>Godina:</b> {{ imdbSlicnog.Year }}</p>
                <p><b>Žanr:</b> {{ imdbSlicnog.Genre }}</p>
                <p><b>Zaplet:</b> {{ imdbSlicnog.Plot }}</p>
                <p><b>Glumci:</b> {{ imdbSlicnog.Actors }}</p>
                <p><b>Redatelj:</b> {{ imdbSlicnog.Director }}</p>
                <p><b>Trajanje:</b> {{ imdbSlicnog.Runtime }}</p>
                <p><b>IMDb ocjena:</b>
                  {{ imdbSlicnog.imdbRating + ' na ' + imdbSlicnog.imdbVotes + ' glasova.' }}</p>
                <b-button class="button" :href="'https://www.imdb.com/title/' + imdbSlicnog.imdbID" target="_blank"
                  variant="warning"><b> Imdb link </b> </b-button>
                <b-button class="button"
                  :href="'https://www.youtube.com/results?search_query=' + imdbSlicnog.Title + ' trailer'"
                  target="_blank" variant="danger"><b> YouTube traileri </b> </b-button>
              </b-card-text>
            </b-card>
          </div>
        </b-modal>
        <b-col cols="4" v-for="film in SlicniFilmovi" v-bind:key="film.imdbID">
          <b-card :title="film.Title" :img-src="film.Poster" img-alt="Image" img-top tag="article"
            style="max-width: 15rem;" class="mb-2">
            <b-card-text>
              <p>Year: {{ film.Year }} </p>
            </b-card-text>

            <b-button v-b-modal.modal-center @click="imdbPodatci(film.imdbID)">Detalji o filmu!</b-button>
          </b-card>
        </b-col>

      </b-row>
      <!--kraj sličnih-->

    </div>
  </b-container>
</template>

<script>
  export default {

    name: "PretragaFilmova",

    data: function () {
      return {
        showModal: false,
        searchText: "",
        imdbSlicnog: [],
        imdb: "",
        PronadeniFilmovi: [],
        SlicniFilmovi: [],

      };
    },
    methods: {
      dohvatiPodatke: function () {
        this.axios.get("http://www.omdbapi.com/?apikey=31de78a9&t=" + this.searchText)
          .then((response) => {
            console.log(response.data)
            this.PronadeniFilmovi = response.data;
            this.imdb = response.data.imdbID;

            if (this.PronadeniFilmovi.Poster == "N/A") {
              this.PronadeniFilmovi.Poster =
                "https://image.shutterstock.com/image-vector/no-image-available-vector-illustration-260nw-744886198.jpg";
            }
          })


        this.axios.get("http://www.omdbapi.com/?apikey=31de78a9&s=" + this.searchText)
          .then((response) => {
            console.log(response.data.Search)
            this.SlicniFilmovi = response.data.Search;



            for (let i = 0; i < this.SlicniFilmovi.length; i++) {
              if (this.SlicniFilmovi[i].Poster == "N/A") {
                this.SlicniFilmovi[i].Poster =
                  "https://image.shutterstock.com/image-vector/no-image-available-vector-illustration-260nw-744886198.jpg";
              }
            }

          })
      },

      imdbPodatci: function (imdb) {
        this.axios.get("http://www.omdbapi.com/?apikey=31de78a9&i=" + imdb)
          .then((response) => {
            console.log(response.data)
            this.imdbSlicnog = response.data;
          })
      },
      modal: function () {

      },
    }
  };
</script>

<style>
  .pronadeno {
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    font-size: 16px;
  }

  .notFound {
    margin-top: 2rem;
  }



  .modal-dialog {
    max-width: 500px;
    margin: 1.75rem auto;
  }
</style>