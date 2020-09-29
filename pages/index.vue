<template>
  <div>
    <v-row align="center" justify="center">
      <v-img
        src="https://cdn.discordapp.com/attachments/392353546332405763/757956807778893954/cropped-animeworld-01.png"
      />
    </v-row>
    <v-row justify="center" class="mt-3 mb-4">
      <v-text-field
        v-model="textSearch"
        hide-details
        label="Filled"
        placeholder="Search Anime"
        filled
        rounded
        dense
        single-line
        class="shrink mx-4"
      />
      <v-btn :color="type === 1 ? 'success' : 'error'" rounded @click="searchData()">
        <v-icon>mdi-magnify</v-icon>
      </v-btn><br><br>
    </v-row>
    <v-card-group columns>
      <v-row justify="center">
        <v-card
          v-for="data in animeData.slice(
            (currentPage - 1) * perPage,
            (currentPage - 1) * perPage + perPage
          )"
          :key="data.mal_id"
          style="width: 450px"
          class="mb-3 ml-4"
        >
          <v-row>
            <v-col md="4" align="center">
              <nuxt-link :to="{ name: 'product-id', params: { id: data } }">
                <v-img :src="data.image_url" />
              </nuxt-link>
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
        <v-pagination v-model="currentPage" :length="6" :total-visible="7" />
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
      perPage: 6,
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
.theme--dark.v-application {
  background-image: url("https://cdn.discordapp.com/attachments/392353546332405763/758277143858774016/704014176bc1f285c0c627b4910b64ae.jpg");
  background-attachment: fixed;
  background-position: 100% 100%;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
