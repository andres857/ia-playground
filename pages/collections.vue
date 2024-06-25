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
        <button class="text-white bg-green-500 hover:bg-green-600 focus:ring-4 focus:outline-none focus:ring-green-300 font-medium rounded-lg text-sm px-3 py-1.5 text-center inline-flex items-center" type="button"> Crear Registros </button>
    </div>
    {{ listVideos }}
    
    <!-- tabla de las colecciones de los clientes creadas -->
    <!-- <div class=" my-10 relative overflow-x-auto shadow-md sm:rounded-lg">
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
                    <td scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap"> {{ video.content_id }} </td>
                    <td scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap"> {{ video.club }} </td>
                    <td scope="row" class="px-6 py-4 font-medium text-gray-900 break-words max-w-xs"> {{ video.url_video}}</td>
                    <td scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap">  <CheckCircleIcon class="size-6 text-green-500"/> <XCircleIcon class="size-6 text-red-500"/></td>
                    <svg aria-hidden="true" role="status" class="inline w-5 h-5 me-3 text-gray-200 animate-spin dark:text-gray-600" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"/>
                        <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="#1C64F2"/>
                    </svg>
                    <CloudArrowUpIcon class="size-6 text-yellow-400"></CloudArrowUpIcon>
                </tr>
            </tbody>
        </table>
    </div>
    {{ listVideos }} -->
</template>
    
<script lang="ts" setup>
    import { ref, onMounted, watch, computed } from 'vue';
    import { initFlowbite, initDropdowns, initModals, initPopovers } from 'flowbite';
    import { BeakerIcon, CheckCircleIcon, XCircleIcon, CloudArrowUpIcon } from '@heroicons/vue/24/solid';
    import axios from 'axios';

    const apiHost = 'http://192.168.0.102:1905';
    const clients = ref([]);
    const isClientsLoaded = ref(false);
    const selectedClient = ref();
    const client = ref([]);
    const listVideos = ref([]);
    const numerodevideos = ref();

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
            // statusUpload.value = error.message;
            console.error(error);
        }
    };

    const selectClient = async (clientData: any) => {
        selectedClient.value = clientData;
        client.value = selectedClient.value.name;
        console.log('Cliente Select', selectedClient.value);
        getContentsVideosFromClientId(selectedClient.value.id);
    }

    const getContentsVideosFromClientId = async (idClient:any) => {
        try {
            const response = await axios.get(`${apiHost}/mzg/client/${idClient}/content/video`);
            if (response.status !== 200) {
                console.log('Error obteniendo la url del contenido');
            } else {
                listVideos.value = response.data;
                numerodevideos.value = listVideos.value.length;
            }
        } catch (error) {
            console.error(error);
        }
    };

    onMounted(async () => {
        initFlowbite();
        initDropdowns();
        initModals();
        initPopovers();
        await getClientsMZG();
    });
</script> 