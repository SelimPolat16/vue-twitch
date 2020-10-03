<template>
  <div id="streamer">
      <topNavbarLinks></topNavbarLinks>

 <!-- Add a placeholder for the Twitch embed -->
    <div id="twitch-embed"></div>
    <div class="streamerInfoContainer">
      <h2 class="streamerName"> {{ $route.params.streamer_user_name }} </h2>
      <h3 class="wiewerCount"> Viewers: {{ this.streamViewerCount }} </h3>
    </div>

<div id="twitch_embed_script_link">
  
</div>

      <h1>{{ this.streamTitle }} </h1>
  </div>
</template>


<script></script>

<script>
import topNavbarLinks from '../components/topNavbarLinks';
export default {
    name: 'Streamer',
    data: function() {
        return{
          streamID: 0,
          streamerName: '',
          gameID: '',
          gameName: '',
          streamTitle: '',
          streamViewerCount: 0,
        }
    },
    components:{
        topNavbarLinks,
    },
    methods: {
       fetchStreamInformation: function () {
         let fetchlink = 'https://api.twitch.tv/helix/channels?broadcaster_id=' + this.$route.query.streamerID;

         fetch(fetchlink, {
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
             this.streamID = data.data[0].broadcaster_id,
          this.streamerName = data.data[0].broadcaster_name,
          this.gameID = data.data[0].game_id,
          this.gameName = data.data[0].game_name,
          this.streamTitle = data.data[0].title
            }
          );
       },
       insertTwitchEmbedScript: function () {
         let twitchEmbedDiv = document.querySelector('#twitch_embed_script_link');
         let twitch_embed_script = document.createElement('script');
         twitch_embed_script.setAttribute('src', 'https://embed.twitch.tv/embed/v1.js');

         twitchEmbedDiv.appendChild(twitch_embed_script);
       },
       runTwitchEmbed: function () {
         var embed = new Twitch.Embed("twitch-embed", {
        width: '100%',
        height: 500,
        channel: this.$route.params.streamer_user_name,
        layout: 'vide-with-chat'
      });

      embed.addEventListener(Twitch.Embed.VIDEO_READY, () =>  {
        var player = embed.getPlayer();
        player.play();
      });
       },
       fetchStreamViewCount: function () {
         fetch('https://api.twitch.tv/helix/streams?user_id=' + this.$route.query.streamerID, {
           method: 'get',
           headers: new Headers ({
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
             this.streamViewerCount = data.data[0].viewer_count;
             console.log(this.streamViewerCount);
           }
         )
       },
       fetchStreamViewCountSetInterval: function () {
         setInterval(() => {
           this.fetchStreamViewCount();
         }, 120000);
       }
    },
    mounted () {
       this.fetchStreamInformation();

      this.insertTwitchEmbedScript();

       setTimeout(() => this.runTwitchEmbed(), 500);

      this.fetchStreamViewCount();
       this.fetchStreamViewCountSetInterval();
    }
}
</script>

<style>
.streamerInfoContainer {
  padding-left: 5%;
  text-align: left;
}
.streamerInfoContainer .streamerName,
.streamerInfoContainer .viewerCount {
  font-weight: 100;
}
</style>







