<template>
    <div>
        <!-- DropDown clients -->
        <div>
            <div id="dropdownHover" class="z-10 hidden bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700">
                <ul class="py-2 text-sm text-gray-700 dark:text-gray-200" aria-labelledby="dropdownHoverButton">
                    <li v-for="client in clients" :key="client.id" @click="selectClient(client)">
                        <a class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">{{ client.name }}</a>
                    </li>
                </ul>
            </div>
            <span>Seleccione el Cliente:</span>
            <button id="dropdownHoverButton" data-dropdown-toggle="dropdownHover" data-dropdown-trigger="hover" class="text-white bg-gray-500 hover:bg-gray-800 focus:ring-4 focus:outline-none focus:ring-gray-300 font-medium rounded-lg text-sm px-3 py-1.5 text-center inline-flex items-center" type="button"> {{ client }} 
                <svg class="w-2.5 h-2.5 ms-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 10 6">
                    <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 4 4 4-4"/>
                </svg>
            </button>
        </div>
    </div>
    <div>
        <button class="text-white bg-green-500 hover:bg-green-600 focus:ring-4 focus:outline-none focus:ring-green-300 font-medium rounded-lg text-sm px-3 py-1.5 text-center inline-flex items-center" type="button" @click="createRecordsForClient"> Crear Registros </button>
    </div>
    {{ listVideosMZG }}
</template>
    
<script lang="ts" setup>
    import { ref, onMounted, watch, computed } from 'vue';
    import { initFlowbite, initDropdowns, initModals, initPopovers } from 'flowbite';
    import { BeakerIcon, CheckCircleIcon, XCircleIcon, CloudArrowUpIcon } from '@heroicons/vue/24/solid';
    import axios from 'axios';

    const apiHost = 'http://186.31.190.89:1905';
    // const apiHost = 'http://192.168.0.102:1905';
    const clients = ref([]);
    const isClientsLoaded = ref(false);
    const selectedClient = ref();
    const client = ref([]);
    const listVideosMZG = ref([]);
    const numerodevideos = ref();
    const records = ref([]);

    const getClientsMZG = async () => {
        try {
            const response = await axios.get(`${apiHost}/mzg/clients`);
            if (response.status !== 200) {
                console.log('Error obteniendo los clients');
            } else {
                clients.value = response.data;
                isClientsLoaded.value = true;
            }
        } catch (error) {
            console.error(error);
        }
    };

    const selectClient = async (clientData: any) => {
        selectedClient.value = clientData;
        client.value = selectedClient.value.name;
        console.log('Cliente Select', selectedClient.value);
        getVideosFromClientId(selectedClient.value.id);
    }

    const getVideosFromClientId = async (idClient:any) => {
        try {
            const response = await axios.get(`${apiHost}/mzg/client/${idClient}/content/video`);
            if (response.status !== 200) {
                console.log('Error obteniendo la url del contenido');
            } else {
                listVideosMZG.value = response.data;
                numerodevideos.value = listVideosMZG.value.length;
            }
        } catch (error) {
            console.error(error);
        }
    };

    const createRecordTranscriptionForVideo = async (video: any) => {
        const payload = {
            id_mzg_customer: video.client_id,
            id_mzg_club: video.club_id,
            id_mzg_content: video.content_id,
            video_url:video.url_video,
            transcription: {
                text: null,
                task: {
                    state: "pending",
                    message: "Transcription queued"
                }
            }
        }
        try {
            console.log('payload Send: ', payload);
            const response = await axios.post(`${apiHost}/transcriptions/video/record`, payload);
            if (response.status !== 200) {
                console.log('Error creando el record para el video');
            }
            console.log(response);
            return response.data;
        } catch (error) {
            console.log(error);
        }
    };

    const createRecordsForClient = async () =>  {
        records.value = await Promise.all(listVideosMZG.value.map(async (video)=>{
            const recordCreated = await createRecordTranscriptionForVideo(video);
            return recordCreated;
        }))
    }

    onMounted(async () => {
        initFlowbite();
        initDropdowns();
        initModals();
        initPopovers();
        await getClientsMZG();
    });
</script> 