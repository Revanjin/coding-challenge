<template>
  <div class="detail">
    <a @click="$router.go(-1)" class="card__planet">Zurück zur Übersicht</a>
    <h1>{{planetInfo.name}}</h1>
    <h3>{{planetInfo.climate}}</h3>
    <h3>{{planetInfo.terrain}}</h3>
  </div>
</template>

<script>
export default {
name:"detail",
  props: {
    test: String,
  },
  data() {
    return{
      planetInfo: []
    }
  },
  created(){
    var url = window.location.hash;
    var index = url.lastIndexOf("/");
    url = url.substr(index + 1)
    console.log(url);

    fetch("https://swapi.dev/api/planets/")
        .then((response) => {
          if (!response.ok) {
            throw Error("error while fetching", url, response);
          }

          return response.json();
        })
        .then((data) => {
          this.planetInfo = data.results.find((planet) => {
            return planet.name == url;
          })
        });
  }
}
</script>