<template>
<div id="streamersForGameContainer">
    <topNavbarLinks></topNavbarLinks>
    <h1>Streamers For  {{ $route.params.game_name }} </h1>
    <div id="topStreamersForSpecificGame">
        <div  class="topStreamerForSpecificGame" v-for='topStreamerForSpecificGame in listOfTopStreamersForSpecificGame' :key='topStreamerForSpecificGame.id'>
            <a :href='`../streamer/${topStreamerForSpecificGame.user_name}?streamerID=${topStreamerForSpecificGame.user_id}`' class="topStreamerForSpecificGameLink">
                
                    <div class="topStreamerForSpecificGameThumbnailContainer">
               <img :src="`${topStreamerForSpecificGame.stream_thumbnail_url}`" alt="" class="topStreamerSpecificGameThumbnail">
           </div >
           <div class="topStreamerForGameDescription">
               <div class="username"> {{ topStreamerForSpecificGame.user_name }} </div>
               <div class="viewerCount"> Viewers: {{ topStreamerForSpecificGame.stream_viewer_count }} </div>
           </div>

            </a>
       
        </div>
    </div>
</div>
</template>

<script>
import topNavbarLinks from '@/components/topNavbarLinks'
export default {
    name: 'StreamersForGame',
    components: {
        topNavbarLinks,
    },
    data: function() {
        return{
            listOfTopStreamersForSpecificGame: [],
        }   
      },
    methods: {
        fetchTopStreamersForSpecificGame: function () {
           let fetchLink = 'https://api.twitch.tv/helix/streams?first=30&game_id=' + this.$route.query.gameID;

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
                        let datalistOfTopStreamersForSpecificGame = [];

                        for (var key in data.data){
                            datalistOfTopStreamersForSpecificGame.push({
                                id: data.data[key].id,
                                user_id: data.data[key].user_id,
                                user_name: data.data[key].user_name,
                                title: data.data[key].title,
                                stream_thumbnail_url: data.data[key].thumbnail_url.replace('{width}','440').replace('{height}','248'),
                                stream_viewer_count: data.data[key].viewer_count,
                            });
                        }

                        this.listOfTopStreamersForSpecificGame = [...this.listOfTopStreamersForSpecificGame, ...datalistOfTopStreamersForSpecificGame];

                    }
                );
        }
    },
    mounted () {
        this.fetchTopStreamersForSpecificGame();
    }
}
</script>

<style>
#topStreamersForSpecificGame {
    display: flex;
    flex-wrap: wrap;
    
}
.topStreamerForSpecificGame{
    flex-basis: 100%;
    padding: 7px;
    margin: 7px;
    margin-bottom: 7px;
    overflow: hidden;
}
.topStreamerSpecificGameThumbnail{
    width: 100%;
}

@media only screen and (min-width: 600px) {
    .topStreamerForSpecificGame {
        flex-basis: 50%;
    }
}
@media only screen and (min-width: 900px) {
    .topStreamerForSpecificGame {
        flex-basis: 33.3333333333%;
    }
}
@media only screen and (min-width: 1200px) {
    .topStreamerForSpecificGame {
        flex-basis: 25%;
    }
}
@media only screen and (min-width: 1500px) {
    .topStreamerForSpecificGame {
        flex-basis: 20%;
    }
}
</style>