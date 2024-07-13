<template>
    <h1>Transcripción Vista</h1>
    <!-- Informacion del contenido -->
    <div>
        <p>Cliente MyZoneGo: {{ transcriptionDetails._id }}</p>
    </div>
    <div>
        <p>Cliente MyZoneGo: {{ transcriptionDetails.id_mzg_customer }}</p>
        <p>Club MyZoneGo: {{ transcriptionDetails.id_mzg_club }}</p>
        <p>Id Contenido: {{ transcriptionDetails.id_mzg_content }}</p>
        <p>Video Url: {{ transcriptionDetails.video_url }}</p>
    </div>
    <!-- Estado de la transcripction -->
    <div>
        <h1> Transcripción </h1>
        <p> ID de la transcripción: {{ transcriptionDetails._id }}</p>
        <p> Estado: {{ transcriptionDetailsTask.state }}</p>
        <p> Mensaje: {{ transcriptionDetailsTask.message }}</p> 
        <p> Fecha de creacion: {{ transcriptionDetails.created_at }} </p>
        <p> Ultima actualizacion: {{ transcriptionDetails.updated_at }}</p>
    </div>
</template>
  
<script lang="ts" setup>
    import axios from 'axios';
    import { ref, onMounted } from 'vue';
    import { useRoute } from 'vue-router';

    const route = useRoute()
    // const apiHost = 'http://192.168.0.102:1905';
    const apiHost = 'http://186.31.190.89:1905';

    const idT = ref()
    const transcriptionDetails = ref([]);
    const transcriptionDetailsTask = ref([]);
    idT.value = route.params.id

    const getTranscriptionById = async (id: any) => {
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
        }
    };
    onMounted(async () => {
        await getTranscriptionById(route.params.id);
    });
</script>
