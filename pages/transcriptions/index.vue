<template>
    <section class="container px-4 mx-auto">
        <div>
            Transcripcion de Videos
        </div>

        <div class="sm:flex sm:items-center sm:justify-between">

            <!-- DropDown clients -->
            <div>
                <span>Seleccione el Cliente:</span>

                <button id="dropdownHoverButton" data-dropdown-toggle="dropdownHover" data-dropdown-trigger="hover" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-3 py-2 text-center inline-flex items-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800" type="button"> {{ client }} 
                    <svg class="w-2.5 h-2.5 ms-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 10 6">
                        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 4 4 4-4"/>
                    </svg>
                </button>
                <div id="dropdownHover" class="z-10 hidden bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700">
                    <ul class="py-2 text-sm text-gray-700 dark:text-gray-200" aria-labelledby="dropdownHoverButton">
                        <li v-for="client in clients" :key="client.id" @click="selectClient(client)">
                            <a class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">{{ client.name }}</a>
                        </li>
                    </ul>
                </div>
            </div>

            <div class="flex items-center mt-4 gap-x-3">
                <button class="flex items-center justify-center w-1/2 px-5 py-2 text-sm tracking-wide text-white transition-colors duration-200 bg-blue-500 rounded-lg shrink-0 sm:w-auto gap-x-2 hover:bg-blue-600 dark:hover:bg-blue-500 dark:bg-blue-600" @click="startTranscription">
                    <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <g clip-path="url(#clip0_3098_154395)">
                        <path d="M13.3333 13.3332L9.99997 9.9999M9.99997 9.9999L6.66663 13.3332M9.99997 9.9999V17.4999M16.9916 15.3249C17.8044 14.8818 18.4465 14.1806 18.8165 13.3321C19.1866 12.4835 19.2635 11.5359 19.0351 10.6388C18.8068 9.7417 18.2862 8.94616 17.5555 8.37778C16.8248 7.80939 15.9257 7.50052 15 7.4999H13.95C13.6977 6.52427 13.2276 5.61852 12.5749 4.85073C11.9222 4.08295 11.104 3.47311 10.1817 3.06708C9.25943 2.66104 8.25709 2.46937 7.25006 2.50647C6.24304 2.54358 5.25752 2.80849 4.36761 3.28129C3.47771 3.7541 2.70656 4.42249 2.11215 5.23622C1.51774 6.04996 1.11554 6.98785 0.935783 7.9794C0.756025 8.97095 0.803388 9.99035 1.07431 10.961C1.34523 11.9316 1.83267 12.8281 2.49997 13.5832" stroke="currentColor" stroke-width="1.67" stroke-linecap="round" stroke-linejoin="round"/>
                        </g>
                        <defs>
                        <clipPath id="clip0_3098_154395">
                        <rect width="20" height="20" fill="white"/>
                        </clipPath>
                        </defs>
                    </svg>

                    <span>Iniciar transcripcion</span>
                </button>
            </div>
        </div>

        <div class="sm:flex sm:items-center sm:justify-between">
            <div>
                <div class="flex items-center gap-x-3">
                    <h2 class="text-lg font-medium text-gray-800 dark:text-white">Videos de la Zona:</h2>
                    <span class="px-3 py-1 text-xs text-blue-600 bg-blue-100 rounded-full dark:bg-gray-800 dark:text-blue-400">{{ numerodevideos }} Videos Transcriptos</span>
                </div>

                <p class="mt-1 text-sm text-gray-500 dark:text-gray-300">Videos transcritos: 8</p>
                <p class="mt-1 text-sm text-gray-500 dark:text-gray-300">Videos restantes: 4</p>
                <p class="mt-1 text-sm text-gray-500 dark:text-gray-300">Videos sin transcripcion: 0</p>

            </div>
            
        </div>
        <!-- transcripcion Actual -->
        <div>
            <h4>Transcripcion Actual</h4>
            <div v-if="currentTranscription">
                <!-- <span>loader</span> -->
                <p>ID: {{ currentTranscription.id_mzg_content }}</p>
                <p>Video: {{ currentTranscription.video_url }}</p>
            </div>
        </div>
        <div class="mt-6 md:flex md:items-center md:justify-between">
            <div class="inline-flex overflow-hidden bg-white border divide-x rounded-lg dark:bg-gray-900 rtl:flex-row-reverse dark:border-gray-700 dark:divide-gray-700">
                <button class="px-5 py-2 text-xs font-medium text-gray-600 transition-colors duration-200 bg-gray-100 sm:text-sm dark:bg-gray-800 dark:text-gray-300">
                    View all
                </button>

                <button class="px-5 py-2 text-xs font-medium text-gray-600 transition-colors duration-200 sm:text-sm dark:hover:bg-gray-800 dark:text-gray-300 hover:bg-gray-100">
                    Monitored
                </button>

                <button class="px-5 py-2 text-xs font-medium text-gray-600 transition-colors duration-200 sm:text-sm dark:hover:bg-gray-800 dark:text-gray-300 hover:bg-gray-100">
                    Unmonitored
                </button>
            </div>

            <div class="relative flex items-center mt-4 md:mt-0">
                <span class="absolute">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mx-3 text-gray-400 dark:text-gray-600">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M21 21l-5.197-5.197m0 0A7.5 7.5 0 105.196 5.196a7.5 7.5 0 0010.607 10.607z" />
                    </svg>
                </span>

                <input type="text" placeholder="Search" class="block w-full py-1.5 pr-5 text-gray-700 bg-white border border-gray-200 rounded-lg md:w-80 placeholder-gray-400/70 pl-11 rtl:pr-11 rtl:pl-5 dark:bg-gray-900 dark:text-gray-300 dark:border-gray-600 focus:border-blue-400 dark:focus:border-blue-300 focus:ring-blue-300 focus:outline-none focus:ring focus:ring-opacity-40">
            </div>
        </div>
        <!-- table -->
        <div class="flex flex-col mt-6">
            <div class="-mx-4 -my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
                <div class="inline-block min-w-full py-2 align-middle md:px-6 lg:px-8">
                    <div class="overflow-hidden border border-gray-200 dark:border-gray-700 md:rounded-lg">
                        <table class="min-w-full divide-y divide-gray-200 dark:divide-gray-700">
                            <thead class="bg-gray-50 dark:bg-gray-800">
                                <tr>
                                    <th scope="col" class="py-3.5 px-4 text-sm font-normal text-left rtl:text-right text-gray-500 dark:text-gray-400">
                                        <button class="flex items-center gap-x-3 focus:outline-none">
                                            <span>ID Contenido</span>

                                            <svg class="h-3" viewBox="0 0 10 11" fill="none" xmlns="http://www.w3.org/2000/svg">
                                                <path d="M2.13347 0.0999756H2.98516L5.01902 4.79058H3.86226L3.45549 3.79907H1.63772L1.24366 4.79058H0.0996094L2.13347 0.0999756ZM2.54025 1.46012L1.96822 2.92196H3.11227L2.54025 1.46012Z" fill="currentColor" stroke="currentColor" stroke-width="0.1" />
                                                <path d="M0.722656 9.60832L3.09974 6.78633H0.811638V5.87109H4.35819V6.78633L2.01925 9.60832H4.43446V10.5617H0.722656V9.60832Z" fill="currentColor" stroke="currentColor" stroke-width="0.1" />
                                                <path d="M8.45558 7.25664V7.40664H8.60558H9.66065C9.72481 7.40664 9.74667 7.42274 9.75141 7.42691C9.75148 7.42808 9.75146 7.42993 9.75116 7.43262C9.75001 7.44265 9.74458 7.46304 9.72525 7.49314C9.72522 7.4932 9.72518 7.49326 9.72514 7.49332L7.86959 10.3529L7.86924 10.3534C7.83227 10.4109 7.79863 10.418 7.78568 10.418C7.77272 10.418 7.73908 10.4109 7.70211 10.3534L7.70177 10.3529L5.84621 7.49332C5.84617 7.49325 5.84612 7.49318 5.84608 7.49311C5.82677 7.46302 5.82135 7.44264 5.8202 7.43262C5.81989 7.42993 5.81987 7.42808 5.81994 7.42691C5.82469 7.42274 5.84655 7.40664 5.91071 7.40664H6.96578H7.11578V7.25664V0.633865C7.11578 0.42434 7.29014 0.249976 7.49967 0.249976H8.07169C8.28121 0.249976 8.45558 0.42434 8.45558 0.633865V7.25664Z" fill="currentColor" stroke="currentColor" stroke-width="0.3" />
                                            </svg>
                                        </button>
                                    </th>

                                    <th scope="col" class="px-4 py-3.5 text-sm font-normal text-left rtl:text-right text-gray-500 dark:text-gray-400">
                                        ID Club
                                    </th>
                                    <th scope="col" class="px-12 py-3.5 text-sm font-normal text-left rtl:text-right text-gray-500 dark:text-gray-400">
                                        Status
                                    </th>
                                    <th scope="col" class="px-12 py-3.5 text-sm font-normal text-left rtl:text-right text-gray-500 dark:text-gray-400">
                                        Mensaje
                                    </th>
                                </tr>
                            </thead>
                            <tbody class="bg-white divide-y divide-gray-200 dark:divide-gray-700 dark:bg-gray-900">
                                <tr v-for="(video, index) in listVideos" :key="index">
                                    <NuxtLink 
                                        :to="{ name: 'transcriptions-id', params:{ id: video._id}}"
                                        class="contents"
                                    >
                                        <td class="px-4 py-4 text-sm font-medium whitespace-nowrap">
                                            <div>
                                                <h2 class="font-medium text-gray-800 dark:text-white">{{ video.id_mzg_content }}</h2>
                                            </div>
                                        </td>
                                        
                                        <td class="px-4 py-4 text-sm whitespace-nowrap">
                                            <div>
                                                <h4 class="text-gray-700 dark:text-gray-200">{{ video.id_mzg_club }}</h4>
                                            </div>
                                        </td>
                                        <td class="px-12 py-4 text-sm font-medium whitespace-nowrap">
                                            <div :class="classStatusTranscription(video.transcription.task.state)">
                                                {{ video.transcription.task.state.charAt(0).toUpperCase() + video.transcription.task.state.slice(1) }}
                                            </div>
                                        </td>
                                        <td class="px-4 py-4 text-sm whitespace-nowrap">
                                            <div>
                                                <h4 class="text-gray-700 dark:text-gray-200">Transcription queued</h4>
                                            </div>
                                        </td>
                                    </NuxtLink>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <div class="mt-6 sm:flex sm:items-center sm:justify-between ">
            <div class="text-sm text-gray-500 dark:text-gray-400">
                Page <span class="font-medium text-gray-700 dark:text-gray-100">1 of 10</span> 
            </div>

            <div class="flex items-center mt-4 gap-x-4 sm:mt-0">
                <a href="#" class="flex items-center justify-center w-1/2 px-5 py-2 text-sm text-gray-700 capitalize transition-colors duration-200 bg-white border rounded-md sm:w-auto gap-x-2 hover:bg-gray-100 dark:bg-gray-900 dark:text-gray-200 dark:border-gray-700 dark:hover:bg-gray-800">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 rtl:-scale-x-100">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M6.75 15.75L3 12m0 0l3.75-3.75M3 12h18" />
                    </svg>

                    <span>
                        previous
                    </span>
                </a>

                <a href="#" class="flex items-center justify-center w-1/2 px-5 py-2 text-sm text-gray-700 capitalize transition-colors duration-200 bg-white border rounded-md sm:w-auto gap-x-2 hover:bg-gray-100 dark:bg-gray-900 dark:text-gray-200 dark:border-gray-700 dark:hover:bg-gray-800">
                    <span>
                        Next
                    </span>

                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 rtl:-scale-x-100">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M17.25 8.25L21 12m0 0l-3.75 3.75M21 12H3" />
                    </svg>
                </a>
            </div>
        </div>
    </section>
</template>
    
<script lang="ts" setup>
    import axios from 'axios';
    import { ref, onMounted, watch, computed } from 'vue';
    import { initFlowbite, initDropdowns, initModals, initPopovers } from 'flowbite';
    import { BeakerIcon, CheckCircleIcon, XCircleIcon, CloudArrowUpIcon } from '@heroicons/vue/24/solid';

    // Definir el tipo para el estado de la transcripción
    type TranscriptionState = 'completed' | 'error' | 'in_progress' | 'pending';

    // const apiHost = 'http://190.27.183.156:1905';
    const apiHost = 'http://192.168.0.102:1905';
    const clients = ref([]);
    const isClientsLoaded = ref(false);
    const selectedClient = ref(null);
    const listVideos = ref([]);
    const client = ref(null);
    const numerodevideos = ref([]);
    const currentTranscription = ref([]);
  
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
        console.log("Video", video);
        try {
            currentTranscription.value = video
            const response = await axios.put(`${apiHost}/transcriptions/video/url`, video, {
                validateStatus: function (status) {
                    return status >= 200 && status < 500; // Considera como éxito cualquier código de estado menor a 500
                }
            });
            if (response.status === 201 || response.status === 404) {
                console.log("RTA transcription");
                console.log(response.data);
                console.log('Estado de la tarea: ', response.data.data.transcription.task.state);
                return response.data.data.transcription.task.state;
            }
        } catch (error) {
            console.error(error);
        }
    }

    const startTranscription = async () =>{
        for (let index = 0; index < listVideos.value.length; index++){
            console.log("index, ", index);
            
            const video = listVideos.value[index];
            listVideos.value[index].transcription.task.state = 'in_progress';

            const statusTranscription = await requestTranscriptionForVideo(video);
            console.log("Peticion transcription video_id: ", video._id)
            listVideos.value[index].transcription.task.state = statusTranscription;   
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

    const classStatusTranscription = computed(() => {
        return (state: TranscriptionState) => {
            switch(state) {
            case 'completed':
                return 'inline px-3 py-1 text-sm font-normal rounded-full text-emerald-500 gap-x-2 bg-emerald-100/60 dark:bg-gray-800';
            case 'error':
                return 'inline px-3 py-1 text-sm font-normal rounded-full text-red-500 gap-x-2 bg-red-100/60 dark:bg-gray-800';
            case 'in_progress':
                return 'inline px-3 py-1 text-sm font-normal rounded-full text-blue-500 gap-x-2 bg-blue-100/60 dark:bg-gray-800';
            case 'pending':
            default:
                return 'inline px-3 py-1 text-sm font-normal rounded-full text-yellow-500 gap-x-2 bg-yellow-100/60 dark:bg-gray-800';
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