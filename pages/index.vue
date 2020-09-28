<template>
  <div class="row text-light">
    <div class="col-sm" align="center">
      <input v-model="textSearch" type="text" placeholder="ค้นหาการ์ตูน">
      <v-btn @click="searchData()">
        Search Anime
      </v-btn><br><br>
      <v-card-group columns justify="space-around">
        <v-row justify="space-around">
          <v-card
            v-for="data in animeData.slice(
              (currentPage - 1) * perPage,
              (currentPage - 1) * perPage + perPage
            )"
            :key="data.mal_id"
            style="width: 450px"
            class="mb-3"
          >
            <v-row>
              <v-col md="4">
                <a :href="data.url">
                  <v-img :src="data.image_url" />
                </a>
                Episode : {{ data.episodes }}
              </v-col>
              <v-col md="8">
                <v-card-body :title="data.title">
                  <v-card-text>{{ data.synopsis }}</v-card-text>
                </v-card-body>
              </v-col>
            </v-row>
          </v-card>
        </v-row>
      </v-card-group>
      <div v-if="run == true">
        <div class="text-center">
          <v-pagination
            v-model="currentPage"
            :length="6"
            :total-visible="7"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      animeData: '',
      textSearch: '',
      perPage: 8,
      currentPage: 1,
      run: false
    }
  },
  methods: {
    searchData () {
      axios
        .get('https://api.jikan.moe/v3/search/anime?q=' + this.textSearch + '')
        .then((response) => {
          this.animeData = response.data.results
          this.run = true
        })
        .catch((err) => {
          console.log(err)
        })
    }
  }
}
</script>

<style>
.card {
  background-color: slategrey;
}
.pagination {
  justify-content: center;
}
body{
  background-color: aliceblue;
}
</style>
