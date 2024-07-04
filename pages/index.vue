<template>
    <div>
        Transcripcion de Videos
    </div>
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
        <button class="text-white bg-green-500 hover:bg-green-600 focus:ring-4 focus:outline-none focus:ring-green-300 font-medium rounded-lg text-sm px-3 py-1.5 text-center inline-flex items-center" type="button" @click="startTranscription"> Iniciar transcripcion </button>
    </div>
    <div>
        <span>Numero de videos: {{ numerodevideos }}</span>                                           
    </div>
    
    <!-- tabla de contenidos -->
    <div class=" my-10 relative overflow-x-auto shadow-md sm:rounded-lg">
        <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
            <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                <tr>
                    <th scope="col" class="px-6 py-3">
                        Id Contenido
                    </th>
                    <th scope="col" class="px-6 py-3">
                        Club
                    </th>
                    <th scope="col" class="px-2 py-3">
                        Url
                    </th>
                    <th scope="col" class="px-6 py-3">
                        Transcripcion
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="video, index in listVideos" :key="index" @click="selectContent(video) " class="bg-white border-b hover:bg-gray-50 dark:hover:bg-gray-600" >
                    <td scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap"> {{ video.id_mzg_content }} </td>
                    <td scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap"> {{ video.id_mzg_club }} </td>
                    <td scope="row" class="px-6 py-4 font-medium text-gray-900 break-words max-w-xs"> {{ video.video_url}}</td>
                    <td scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap">
                    <component :is="statusTranscription(video.transcription.task.state)" :class="getStatusColor(video.transcription.task.state)" />
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    {{ listVideos }}
</template>
    
<script lang="ts" setup>
    import { ref, onMounted, watch, computed } from 'vue';
    import { initFlowbite, initDropdowns, initModals, initPopovers } from 'flowbite';
    import { BeakerIcon, CheckCircleIcon, XCircleIcon, CloudArrowUpIcon } from '@heroicons/vue/24/solid';
    import axios from 'axios';

    // Definir el tipo para el estado de la transcripción
    type TranscriptionState = 'completed' | 'error' | 'in_progress' | 'pending';

    // const apiHost = 'http://186.31.190.89:1905';
    const apiHost = 'http://192.168.0.102:1905';
    const clients = ref([]);
    const isClientsLoaded = ref(false);
    const selectedClient = ref(null);
    const listVideos = ref([]);
    const client = ref(null);
    const numerodevideos = ref([]);
  
    const getClients = async () => {
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
        getVideosFromClientId(selectedClient.value.id);
    }

    const getVideosFromClientId = async (idClient:any) => {
        try {
            const response = await axios.get(`${apiHost}/transcriptions/client/${idClient}`);
            if (response.status !== 200) {
                console.log('Error obteniendo la transcripcion del video');
            } else {
                listVideos.value = response.data;
                numerodevideos.value = listVideos.value.length;
            }
        } catch (error) {
            console.error(error);
        }
    };

    const requestTranscriptionForVideo = async (video: any) =>{
        console.log("video", video);
        try {
            const response = await axios.put(`${apiHost}/transcriptions/video/url`,
                video
            );
            if (response.status !== 200) {
                console.log('Error actualizando la transcripcion del video');
            } else {
                listVideos.value = response.data;
                numerodevideos.value = listVideos.value.length;
            }
        } catch (error) {
            console.error(error);
        }
    }

    const startTranscription = async () =>{
        for (let index = 0; index < listVideos.value.length; index++){
            const video = listVideos.value[index];
            listVideos.value[index].transcription.task.state = 'in_progress';

            const response = await requestTranscriptionForVideo(video);
            console.log("peticion transcription")
            if (response.status !== 200) {
                console.log('Error actualizando la transcripcion del video');
            } else {
                listVideos.value[index].transcription.task.state = response.data.transcription.task.state;   
            }
        }
    }

    // const selectContent = async (contentVideo: any) => {
    //     console.log('contenido Seleccionado', contentVideo);
    //     // await createRecordTranscriptionForVideo(contentVideo);
    // }
    
    const listContentByDefault = async () => {
        await getClients();
        selectedClient.value = clients.value[1];
        await selectClient(selectedClient.value);
    }

    const statusTranscription = computed(() => {
        return (state: TranscriptionState) => {
            switch(state) {
            case 'completed':
                return CheckCircleIcon;
            case 'error':
                return XCircleIcon;
            case 'in_progress':
                return BeakerIcon;
            case 'pending':
                default:
                    return CloudArrowUpIcon;
            }
        };
    });

    const getStatusColor = computed(() => {
        return (state: TranscriptionState) => {
            switch(state) {
            case 'completed':
                return 'text-green-500 size-7';
            case 'error':
                return 'text-red-500 size-7';
            case 'in_progress':
                return 'text-red-500 size-7';
            case 'pending':
            default:
                return 'text-yellow-400 size-7';
            }
        };
    });

    onMounted(async () => {
        initFlowbite();
        initDropdowns();
        initModals();
        initPopovers();
        // await getClients();
        await listContentByDefault();
    });
</script> 