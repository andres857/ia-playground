<template>
    <video
        class=" video-js"
        controls
        preload="auto"
        data-setup="{}"
    >
        <source :src="urlVideo" type="video/mp4" />
    </video>
</template>

<script lang="ts" setup>
    import videojs from 'video.js';
    import 'video.js/dist/video-js.css';

    const props = defineProps({
        urlPlayer: String,
    });

    const urlVideo = ref()

    onMounted(() => {
        urlVideo.value = props.urlPlayer;
        console.log(urlVideo.value);
    })
</script>

<style scoped>
    .video-js {
        width: 100%;
        height: 100%;
    }
</style>