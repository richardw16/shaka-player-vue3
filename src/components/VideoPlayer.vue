<template>
  <div ref="videoContainer" class="shadow-lg mx-auto max-w-full size shaka-video-container">
    <video
      id="video"
      ref="videoPlayer"
      class="w-full h-full shaka-video"
      :poster="posterUrl"
    ></video>
   
    <div ref="subtitleDisplay"></div>
  </div>

</template>

<script setup>
import { onMounted, ref } from '@vue/runtime-core';
const videoContainer = ref(null);
const videoPlayer = ref(null);
const subtitleDisplay = ref(null);
const props = defineProps({
    manifestUrl: {
      type: String,
      required: true
    },
    licenseServer: {
      type: String,
      required: true
    },
    posterUrl: {
      type: String,
      required: false,
      default: ''
    },
    subTitles: {
      type: Array,
      required: false,
      default: []
    }
});
const onError = (error) => {
  console.error('Error code', error.code, 'object', error);
};
onMounted(async () => {
  const shaka = await import('shaka-player/dist/shaka-player.ui.js');
  

  const player = new shaka.Player(videoPlayer.value);
    const ui = new shaka.ui.Overlay(
      player,
      videoContainer.value,
      videoPlayer.value
    );
    ui.getControls();

    console.log(Object.keys(shaka.ui));
    if(props.licenseServer && props.licenseServer.length > 0){
      player.configure({
        drm: {
          servers: { 'com.widevine.alpha': props.licenseServer }
        }
      });
    }

    player
      .load(props.manifestUrl)
      .then(() => {
        
        console.log('The video has now been loaded!');

        for (const s of props.subTitles) {
          player
              .addTextTrackAsync(s.path, s.short, 'subtitles', 'text/vtt')
              .then(function () {
                console.log('Subtitle track added successfully.');
              });
        }
        // player
        //       .addTextTrackAsync('/v2/sub_en.vtt', 'en', 'subtitles', 'text/vtt')
        //       .then(function () {
        //         console.log('Subtitle track added successfully.');
        //       });
        // player
        //       .addTextTrackAsync('/v2/sub_nl.vtt', 'nl', 'subtitles', 'text/vtt')
        //       .then(function () {
        //         console.log('Subtitle track added successfully.');
        //       });
         
        
      })
      .catch(onError); // onError is executed if the asynchronous load fails.
      // Get the list of available text tracks
      const tracks = player.getTextTracks();
      console.log(tracks);
    // Add the VTT subtitle track
    
      // player.selectTextTrack(tracks[0]);
});
</script>



<style>
@import '../../node_modules/shaka-player/dist/controls.css'; /* Shaka player CSS import */

.size {
  width: 800px;
}
</style>
