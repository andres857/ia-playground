<template>
    <div class="mb-3">
        <div class="input-group">
          <span class="input-group-text" id="basic-addon3">URL</span>
          <input type="text" class="form-control" row=2 id="basic-url" v-model.trim="urlVideo" aria-describedby="basic-addon3 basic-addon4">
          <button class="px-4 py-2 font-bold text-white bg-blue-500 rounded hover:bg-blue-700 focus:outline-none focus:shadow-outline disabled:opacity-50 disabled:cursor-not-allowed" :disabled="!videoIsValid">
            Obtener Transcripción
          </button>
        </div>
        <div class="form-text" id="basic-addon4">Ingrese la url del video</div>
    </div>
    
    <!-- preview Video  -->
    <div class="row justify-content-center">
        <div v-show="videoIsValid" >
            <div class=" align-items-center justify-content">
                <span>Porcentaje visto del video: {{progressVideoPorcen}} %</span>
                <span>Porcentaje requerido: {{requiredPercentage}} %</span>
            </div>
            <div class="col-12">
                <video
                ref="videoSource"
                    id="my-video"
                    class="video-js"
                    controls
                    preload="auto"
                    width="720"
                    height="264"
                    data-setup="{}"
                    >
                    <source :src="processedUrl" type="video/mp4" />
                </video>
            </div>
            <div class="col-12 my-2">
                <button class="btn btn-success" :disabled="!pogressVideoComplete">Siguiente</button>
            </div>
        </div>
        <div v-show="!videoIsValid && processedUrl !== false" class="row justify-content-center align-items-center">
            <div class="col-auto">
                <svg xmlns="http://www.w3.org/2000/svg"
                    class="bi bi-exclamation-triangle-fill flex-shrink-0 me-2 text-danger" style="width: 3rem; height: 3rem;" fill="#000000" viewBox="0 0 16 16"
                    role="img" aria-label="Warning:">
                    <path
                        d="M8.982 1.566a1.13 1.13 0 0 0-1.96 0L.165 13.233c-.457.778.091 1.767.98 1.767h13.713c.889 0 1.438-.99.98-1.767L8.982 1.566zM8 5c.535 0 .954.462.9.995l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 5.995A.905.905 0 0 1 8 5zm.002 6a1 1 0 1 1 0 2 1 1 0 0 1 0-2z" />
                </svg>
            </div>
            <div class="col-auto">
                <h3>Ingrese una url valida para visualizar el Video.</h3> 
            </div>
        </div>
    </div>
</template>

<script lang="ts" setup>
    import videojs from 'video.js';
    import 'video.js/dist/video-js.css';

    const urlVideo = ref('');
    const videoSource = ref(null);
    const playerState = ref(null);
    const playerEvents = ref([]);
    const progressVideoPorcen = ref(0);
    const updateCount = ref(0);
    const requiredPercentage = 10; // Porcentaje requerido para avanzar
    const requiredUpdates = ref(0);
    const videoReady = ref(false);

    const processedUrl = computed(() => {
        try {
            const url = new URL(urlVideo.value)
            console.log(url);
            return url.href
        } catch (error) {
            return false
        }
    })

    const videoIsValid = computed(()=> videoReady.value === true && processedUrl.value !== false );

    watch(processedUrl, (newUrl) => {
        if (videoSource.value) {
            videoSource.value.src = newUrl
        }
    })

    const resetProgressVideo = () => {
        updateCount.value = 0;
        progressVideoPorcen.value = 0;
    }

    const initVideoPlayer = () => {
        if (videoSource.value) {
            const player = videojs(videoSource.value, {}, function() {
                // eventos del reproductor
                this.on('loadedmetadata', () => {
                    const duration = this.duration();
                    console.log('event load video');
                    resetProgressVideo();
                    videoReady.value = true;
                    requiredUpdates.value = calculateRequiredUpdates(duration);
                })

                this.on('timeupdate', () => {
                    updateCount.value++
                    const currentPercentage = (updateCount.value / requiredUpdates.value) * 100
                    progressVideoPorcen.value = currentPercentage.toFixed();
                    playerEvents.value.push(playerState.value);
                })

                this.on('error', (error) => {
                    videoReady.value = false;
                    playerState.value = 'error';
                    playerEvents.value.push('error');
                    console.error('Error al cargar el video:', error);
                })

                this.on('ended', () => {
                    playerState.value = 'ended'
                    playerEvents.value.push('ended')
                })                        
            })
        }   
    }

    const calculateRequiredUpdates = (duration) => {
        const totalUpdates = Math.floor(duration * 4) // Aproximadamente 4 actualizaciones por segundo
        // return Math.ceil((totalUpdates * requiredPercentage) / 100) // retorna el porcentaje 
        return Math.ceil(totalUpdates) // retorna el porcentaje 
    }

    onMounted(()=>{
        initVideoPlayer()
    })
</script>