<template>
    <div class="grid grid-cols-12 grid-rows-8 gap-4 bg-blue-300 min-h-screen">
        <div class="col-span-6 col-start-2 row-start-2 bg-slate-100">Nombre del contenido</div>
        <div class="col-span-3 col-start-9 row-start-2">Transcriptor</div>
        <div v-if="isLoading"> Cargando....</div>
        <div v-else class="col-span-6 row-span-3 col-start-2 row-start-3 bg-orange-500">
            <div class="grid gap-2 w-full h-full">
                <VideoPlayer :urlPlayer="transcriptionDetails.video_url"/>
            </div>
        </div>
        <div class="col-span-3 col-start-9 row-start-3">es | en</div>
        <div class="col-span-3 row-span-5 col-start-9 row-start-4">Contenido</div>
        <div class="col-span-6 col-start-2 row-start-6">Acciones</div>
        <div class="col-span-6 row-span-2 col-start-2 row-start-7">Iconos</div>
    </div>
<!--     
    <h1>Transcripción Vista</h1>
    Informacion del contenido
    <div>
        <p>Cliente MyZoneGo: {{ transcriptionDetails._id }}</p>
    </div>
    <div>
        <p>Cliente MyZoneGo: {{ transcriptionDetails.id_mzg_customer }}</p>
        <p>Club MyZoneGo: {{ transcriptionDetails.id_mzg_club }}</p>
        <p>Id Contenido: {{ transcriptionDetails.id_mzg_content }}</p>
        <p>Video Url: {{ transcriptionDetails.video_url }}</p>
    </div>
    Estado de la transcripction
    <div>
        <h1> Transcripción </h1>
        <p> ID de la transcripción: {{ transcriptionDetails._id }}</p>
        <p> Estado: {{ transcriptionDetailsTask.state }}</p>
        <p> Mensaje: {{ transcriptionDetailsTask.message }}</p> 
        <p> Fecha de creacion: {{ transcriptionDetails.created_at }} </p>
        <p> Ultima actualizacion: {{ transcriptionDetails.updated_at }}</p>
    </div> -->
</template>
  
<script lang="ts" setup>
    import axios from 'axios';
    import { ref, onMounted } from 'vue';
    import { useRoute } from 'vue-router';
    import VideoPlayer from '~/components/VideoPlayer.vue';

    const route = useRoute()
    // const apiHost = 'http://192.168.0.102:1905';
    const apiHost = 'http://190.27.183.156:1905';

    const idT = ref()
    const isLoading = ref(true);
    const transcriptionDetails = ref([]);
    const transcriptionDetailsTask = ref([]);
    idT.value = route.params.id

    const getTranscriptionById = async (id: any) => {
        isLoading.value = true;
        try {
            const response = await axios.get(`${apiHost}/transcriptions/${id}`);
            if (response.status !== 200) {
                console.log('Error obteniendo los clients');
            } else {
                transcriptionDetails.value = response.data.data.content;
                transcriptionDetailsTask.value = response.data.data.content.transcription.task;                
            }
        } catch (error) {
            console.error(error);
        } finally {
            isLoading.value = false;
        }
    };
    onMounted(async () => {
        await getTranscriptionById(route.params.id);
    });
</script>
