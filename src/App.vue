<template>
  <!--class="col-sm-12 text-center"-->
  <div class="p-3 mb-2 bg-light text-dark">
    <v-container align-center>
      <h1>Explore the world...</h1>
      <div class="alert alert-primary" role="alert">Clic here to select a country</div>

      <div class="shadow p-3 mb-5 bg-white rounded">
        <select class="form-control" v-model="select" id="exampleFormControlSelect1">
          <option
            v-for="country in countries"
            :key="country.id"
            :value="{ id: country.id, text: country.name }"
          >{{country.name}}</option>
        </select>
      </div>

      <div v-for="country in countries" :key="country.id">
        <div v-if="select.text === country.name">
         
          <h2> {{country.name}}</h2>
          
          <h3>Capital: {{country.capital}}</h3>
          <br>
          <h3>Population: {{country.population}}</h3>
          <br>
          <h3>Continent: {{country.region}}</h3>
          <br>
          <h3>Region: {{country.subregion}}</h3>
          <br>

          <h3>
            Flag:
            <img :src="country.flag" width="90px">
          </h3>

          <br>

          <div id="map" class="map">
            <l-map :zoom="zoom" :center="latLng(country.latlng[0],country.latlng[1])">
              <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
              <l-marker :lat-lng="latLng(country.latlng[0],country.latlng[1])"></l-marker>
            </l-map>
          </div>
        </div>
      </div>
    </v-container>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker } from "vue2-leaflet";
import axios from "axios";
export default {
  data() {
    return {
      select: "",
      countries: [],
      zoom: 13,
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
    };
  },
  components: {
    LMap,
    LTileLayer,
    LMarker
  },

  created() {
    var that = this;
    axios
      .get("https://restcountries.eu/rest/v2/all")
      .then(res => {
        that.countries = res.data;
        console.log(res.data);
      })
      .catch(error => console.log(error));
  },

  methods: {
    latLng: function(lat, log) {
      return L.latLng(lat, log);
    }
  }
};
</script>


 <style scoped>
h3 {
  color: #666666;
}
h2 {
  color:  steelblue;
}

h1 {
  color: steelblue;
}

#app {
  background-color: beige;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 90px;
}

.center {
  align-content: center;
}

.map {
  height: 700px;
  width: 1725px;
  align-content: center;
  background: WhiteSmoke;
}
</style>



































































