<template>
  <div id="GetTopGamesContainer">
      <topNavbarLinks></topNavbarLinks>
      <h1>List of Top Games</h1>
      <div id="topGames">
          <div class="topGame" v-for="topGame in listOfTopGames" :key="topGame.gameID">
              <a href="">
                <div class="topGameThumbnailContainer">
                  <img class='topGameThumbnail' :src="`${topGame.gameBoxArtURL}`"  alt="">
                </div>
                <div class="topGameDescription">
                  {{ topGame.gameName }}
              </div>
              </a>
              
          </div>
      </div>
  </div>
</template>

<script>
import topNavbarLinks from '../components/topNavbarLinks';
export default {
    name: 'GetTopGames',
    data: function() {
        return{
            listOfTopGames: [],
        }
    },
    components:{
        topNavbarLinks,
    },
    methods: {
       fetchTopGames: function () {
           let fetchLink = 'https://api.twitch.tv/helix/games/top?first=30';

           fetch(fetchLink, {
               method: 'get',
               headers: new Headers({
                   'Authorization': 'Bearer aadu4kz6l9nhypz12efbij3ld5mp4d',
                   'Client-ID': 'rnfu5mx7fvhrnyuzkkoskjqvqmz0tj',
               })
           })
           .then(
               function (response) {
                   return response.json();
               }
           )
           .then(
               data => {
                   console.log(data);

                   let dataListOfTopGames = [];

                   for (var key in data.data) {
                       dataListOfTopGames.push({
                            gameID: data.data[key].id,
                            gameName: data.data[key].name,
                            gameBoxArtURL: data.data[key].box_art_url.replace('{width}','213').replace('{height}', '285'),
                       })
                   }

                   this.listOfTopGames = [...this.listOfTopGames, ...dataListOfTopGames]
               }
           );
       }
    },
    mounted () {
        this.fetchTopGames();
    }
}
</script>

<style>
#topGames{
    display: flex;
    flex-wrap: wrap;

}
.topGame{
    flex-basis: 100%;
    padding: 7px; 
    margin-bottom: 7px;
}
.topGameThumbnail {
    width: 100%;
}

@media only screen and (min-width: 350px) {
    .topGame{
        flex-basis: 50%;
    }
}
@media only screen and (min-width: 650px) {
    .topGame{
        flex-basis: 33.33333333333%;
    }
}
@media only screen and (min-width: 950px) {
    .topGame{
        flex-basis: 25%;
    }
}
@media only screen and (min-width: 1250px) {
    .topGame{
        flex-basis: 20%;
    }
}
@media only screen and (min-width: 1550px) {
    .topGame{
        flex-basis: 16.66666666666667%;
    }
}
@media only screen and (min-width: 1850px) {
    .topGame{
        flex-basis: 14.2857142857%;
    }
}
</style>