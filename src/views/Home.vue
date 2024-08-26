<template>
    <div>
      <VideoPlayer
        :licenseServer="licenseServer"
        :manifestUrl="manifestUrl"
        :posterUrl="posterUrl"
        :subTitles="subTitles"
      />
    </div>
  </template>
  
  <script setup>
  import VideoPlayer from '@/components/VideoPlayer.vue';
  
  const licenseServer = '';
  const manifestUrl = 'https://richardw16azuredemo.blob.core.windows.net/aid/master.m3u8';
  const posterUrl = 'https://richardw16azuredemo.blob.core.windows.net/aid/thumbnail.jpg';
  const subTitles = [{path:'https://richardw16azuredemo.blob.core.windows.net/aid/sub_en.vtt', short: 'en'},{path:'https://richardw16azuredemo.blob.core.windows.net/aid/sub_nl.vtt', short: 'nl'}] 
  </script>
  
  <style>
    .container {
    margin: 0 auto;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    }

    .mx-auto {
    margin-left: auto;
    margin-right: auto;
    }

    .shadow-lg {
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1),
        0 4px 6px -2px rgba(0, 0, 0, 0.05);
    }

    .max-w-full {
    max-width: 100%;
    }

    .w-full {
    width: 100%;
    }

    .h-full {
    height: 100%;
    }
  </style>