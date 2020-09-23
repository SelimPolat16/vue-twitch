<template>
  <div id="topStreamersContainer">
      <topNavbarLinks></topNavbarLinks>
      <h1>List of Top Stramers</h1>
      <div id="topStreamers">
          <div class="topStreamer" v-for="topStreamer in listOfTopStreamers" :key="topStreamer.id">
              <a href="">
                  <div class="topStreamerLink">
                      <div class="topStreamerThumbnailContainer">
                          <img class='topStreamerThumbnail' :src="`${topStreamer.thumbnail_url}`" alt="">
                      </div>
                      <div class="topStreamerDescription">
                          <span class="streamTitle">
                              <strong> {{ topStreamer.stream_title  }} </strong></span>
                              <span class="username">Streamer Name: {{ topStreamer.user_name }} </span>
                              <span class="streamViewerCount">Viwers: {{ topStreamer.viewer_count }} </span>
                      </div>
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
    components:{
        topNavbarLinks,
    },
   data: function () {
       return {
           listOfTopStreamers: [],    
       }
   },
   methods: {
       fetchTopStreamers: function () {
           let fetchLink = 'https://api.twitch.tv/helix/streams?first=30';

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
                   // console.log(data);
                   let dataListOfTopStreamers = [];

                   for (var key in data.data) {
                       dataListOfTopStreamers.push({
                            id: data.data[key].id,
                            user_id: data.data[key].user_id,
                            user_name: data.data[key].user_name,
                            game_id: data.data[key].game_id,
                            stream_title: data.data[key].title,
                            viewer_count: data.data[key].viewer_count,
                            thumbnail_url: data.data[key].thumbnail_url.replace('{width}','440').replace('{height}', '248'),
                       });
                   }

                   this.listOfTopStreamers = [...this.listOfTopStreamers, ...dataListOfTopStreamers];
                }
            );
       }
   },
   mounted () {
       this.fetchTopStreamers();
   }
}
</script>

<style>
#topStreamers{
    display: flex;
    flex-wrap: wrap;
}
.topStreamer {
  /*  flex-basis: 100%; */
  overflow: hidden;
}
.topStreamerThumbnail {
    width: 100%;
}
.topStreamerDescription span {
    display: block;
}
</style>