<template>
    <section v-if="isLoading" class=" grid bg-slate-200 min-h-screen place-content-center">
        <div role="status" class="flex gap-2 justify-center items-center">
            <svg aria-hidden="true" class="w-8 h-8 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"/>
                <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentFill"/>
            </svg>
            <span class=" text-lg">Cargando...</span>
        </div>
    </section>

    <section v-else class="grid grid-cols-12 justify-items-stretch min-h-screen gap-x-10 bg-slate-200">
        <div class="col-span-7 justify-self-end">
                <div class="grid content-center">
                    <!-- description transcription -->
                    <div class="h-[25vh] flex-col w-full items-start justify-center py-4">
                        <h1 class="font-bold text-lg">Estado de la Transcripción</h1>
                        <div class="flex gap-4 w-[750px] text-sm mt-2 justify-center items-center">
                            <div class="flex flex-col w-4/12">
                                <p>ID Contenido Zona Privada: {{ transcriptionDetails.id_mzg_content }}</p>
                                <p> ID de la Transcripción: {{ transcriptionDetails._id }}</p>
                            </div>
                            <div class="flex flex-col w-8/12">
                                <p>Estado: {{ transcriptionDetailsTask.state }}</p>
                                <div class="flex overflow-y-auto gap-2">
                                    <p class=" font-medium"> Mensaje: </p>
                                    <span> {{ transcriptionDetailsTask.message }}</span>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- media player -->
                    <div class="h-[55vh] flex items-center justify-center">
                        <div class="h-full">
                            <VideoPlayer :urlPlayer="transcriptionDetails.video_url"/>
                        </div>
                    </div>
                    <!-- Aciones -->
                    <div class=" h-[20vh] overflow-hidden">
                        <div class="flex gap-2 items-start justify-center mt-5 h-full">
                            <div v-for="item in inferencesDetails" :key="item._id">
                                <button type="button" :class="['flex flex-col items-center justify-center', ...getActionsClasses]"
                                @click="chooseAction(item)">
                                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6 px-auto focus:text-blue-700">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M7.5 8.25h9m-9 3H12m-9.75 1.51c0 1.6 1.123 2.994 2.707 3.227 1.129.166 2.27.293 3.423.379.35.026.67.21.865.501L12 21l2.755-4.133a1.14 1.14 0 0 1 .865-.501 48.172 48.172 0 0 0 3.423-.379c1.584-.233 2.707-1.626 2.707-3.228V6.741c0-1.602-1.123-2.995-2.707-3.228A48.394 48.394 0 0 0 12 3c-2.392 0-4.744.175-7.043.513C3.373 3.746 2.25 5.14 2.25 6.741v6.018Z" />
                                    </svg>
                                    <p class="text-sm text-gray-700 pt-1">{{ item.rol }}</p>
                                    
                                </button>
                            </div>
                            <!-- <button type="button" :class="['flex flex-col items-center justify-center', ...getActionsClasses]">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6 px-auto focus:text-blue-700">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M7.5 8.25h9m-9 3H12m-9.75 1.51c0 1.6 1.123 2.994 2.707 3.227 1.129.166 2.27.293 3.423.379.35.026.67.21.865.501L12 21l2.755-4.133a1.14 1.14 0 0 1 .865-.501 48.172 48.172 0 0 0 3.423-.379c1.584-.233 2.707-1.626 2.707-3.228V6.741c0-1.602-1.123-2.995-2.707-3.228A48.394 48.394 0 0 0 12 3c-2.392 0-4.744.175-7.043.513C3.373 3.746 2.25 5.14 2.25 6.741v6.018Z" />
                                </svg>
                                <p class="text-sm text-gray-700 pt-1">Transcripcion</p>
                            </button> -->

                            <!-- <button type="button" class=" flex flex-col items-center justify-center px-4 py-3 text-sm font-medium text-gray-900 bg-white border border-gray-200 rounded-s-lg rounded-lg shadow hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 14.25v-2.625a3.375 3.375 0 0 0-3.375-3.375h-1.5A1.125 1.125 0 0 1 13.5 7.125v-1.5a3.375 3.375 0 0 0-3.375-3.375H8.25m0 12.75h7.5m-7.5 3H12M10.5 2.25H5.625c-.621 0-1.125.504-1.125 1.125v17.25c0 .621.504 1.125 1.125 1.125h12.75c.621 0 1.125-.504 1.125-1.125V11.25a9 9 0 0 0-9-9Z" />
                                </svg>
                                <p class="text-sm text-gray-900 pt-1 ">Resumen</p>
                            </button> -->

                            <!-- <button type="button" class=" flex flex-col items-center justify-center px-4 py-3 text-sm font-medium text-gray-900 bg-white border border-gray-200 rounded-s-lg rounded-lg shadow hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M9 12h3.75M9 15h3.75M9 18h3.75m3 .75H18a2.25 2.25 0 0 0 2.25-2.25V6.108c0-1.135-.845-2.098-1.976-2.192a48.424 48.424 0 0 0-1.123-.08m-5.801 0c-.065.21-.1.433-.1.664 0 .414.336.75.75.75h4.5a.75.75 0 0 0 .75-.75 2.25 2.25 0 0 0-.1-.664m-5.8 0A2.251 2.251 0 0 1 13.5 2.25H15c1.012 0 1.867.668 2.15 1.586m-5.8 0c-.376.023-.75.05-1.124.08C9.095 4.01 8.25 4.973 8.25 6.108V8.25m0 0H4.875c-.621 0-1.125.504-1.125 1.125v11.25c0 .621.504 1.125 1.125 1.125h9.75c.621 0 1.125-.504 1.125-1.125V9.375c0-.621-.504-1.125-1.125-1.125H8.25ZM6.75 12h.008v.008H6.75V12Zm0 3h.008v.008H6.75V15Zm0 3h.008v.008H6.75V18Z" />
                                </svg>  
                                <p class="text-sm text-gray-900 pt-1 ">Ideas clave</p>
                            </button> -->

                            <!-- <button type="button" class=" flex flex-col items-center justify-center px-4 py-3 text-sm font-medium text-gray-900 bg-white border border-gray-200 rounded-s-lg rounded-lg shadow hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M8.242 5.992h12m-12 6.003H20.24m-12 5.999h12M4.117 7.495v-3.75H2.99m1.125 3.75H2.99m1.125 0H5.24m-1.92 2.577a1.125 1.125 0 1 1 1.591 1.59l-1.83 1.83h2.16M2.99 15.745h1.125a1.125 1.125 0 0 1 0 2.25H3.74m0-.002h.375a1.125 1.125 0 0 1 0 2.25H2.99" />
                                </svg>
                                <p class="text-sm text-gray-900 pt-1 ">Preguntas</p>
                            </button> -->
                        </div>
                    </div>
                </div>
        </div>
        <!-- Zona de resumenes del video -->
        <div class="col-span-5 h-[100vh] overflow-hidden">
            <div class="flex flex-col h-full space-y-4 overflow-hidden pr-28">

                <div class="flex justify-evenly">
                    <div class="flex items-center justify-center">
                        <h3 class="text-lg font-semibold"> Transcripcion </h3>
                    </div>
                    <div class="flex items-center justify-center rounded-md shadow-sm h-[10vh]" role="group">
                        <button type="button" @click="selectLanguage('ES')" 
                        :class="['rounded-s-lg', ...getButtonClassesLanguage('ES')]">
                            ES
                        </button>
                        <button type="button" @click="selectLanguage('EN')" 
                            :class="['rounded-e-lg', ...getButtonClassesLanguage('EN')]">
                            EN
                        </button>
                    </div>
                </div>
                <div class="flex-grow overflow-y-auto bg-white rounded-lg shadow-inner p-4">
                    <!-- <p class=" text-md">{{ transcriptionDetails.transcription.text }}</p> -->
                    <p class=" text-md">{{ contentToShow }}</p>
                </div>
                <div class="flex justify-end">
                    <button type="button" class="text-white bg-blue-700 hover:bg-blue-800 focus:outline-none focus:ring-4 focus:ring-blue-300 font-medium rounded-full text-sm px-5 py-2.5 text-center me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Actualizar</button>
                </div>
            </div>
        </div>
    </section>
    {{ transcriptionDetails }}
</template>
  
<script lang="ts" setup>
    import { useRuntimeConfig } from '#app';


    import { ref, onMounted,computed } from 'vue';
    import { useRoute } from 'vue-router';
    import axios from 'axios';
    import VideoPlayer from '~/components/VideoPlayer.vue';

    const route = useRoute();
    const config = useRuntimeConfig();
    const apiHost = config.public.apiBase;

    const idT = ref()
    const isLoading = ref(true);
    const transcriptionDetails = ref([]);
    const inferencesDetails = ref();
    const transcriptionDetailsTask = ref([]);
    const contentToShow = ref();
    idT.value = route.params.id

    const selectedLanguage = ref('ES'); // Idioma por defecto
    const selectedAction = ref(null)

    const getTranscriptionById = async (id: any) => {
        isLoading.value = true;
        try {
            const response = await axios.get(`${apiHost}/transcriptions/${id}`);
            if (response.status !== 200) {
                console.log('Error obteniendo los clients');
            } else {
                transcriptionDetails.value = response.data.data.content;
                contentToShow.value = response.data.data.content.transcription.text;

                transcriptionDetailsTask.value = response.data.data.content.transcription.task;                
            }
        } catch (error) {
            console.error(error);
        } finally {
            isLoading.value = false;
        }
    };

    const getInferencesByIdVideoTranscription = async (id: any) => {
        isLoading.value = true;
        try {
            const response = await axios.get(`${apiHost}/inference/videotranscription/${id}`);
            if (response.status !== 200) {
                console.log('Error obteniendo las Inferencias');
            } else {
                console.log(response.data);
                inferencesDetails.value = response.data.inferences;
            }
        } catch (error) {
            console.error(error);
        } finally {
            isLoading.value = false;
        }
    };

    const selectLanguage = (lang: string) => {
        selectedLanguage.value = lang;
        console.log(selectedLanguage.value);
        contentToShow.value = selectedAction.value.text.es;
        if (selectedLanguage.value === "ES"){
            contentToShow.value = selectedAction.value.text.es;
        } else if (selectedLanguage.value === "EN"){
            contentToShow.value = selectedAction.value.text.en;
        }
    };

    const chooseAction = (action: any) => {
        selectedAction.value = action
        contentToShow.value = selectedAction.value.text.es;
        console.log(selectedAction.value);
    };

    const getButtonClassesLanguage = computed(() =>( lang:string ) => {
        const baseClasses = 'px-4 py-2 text-sm font-medium border border-gray-200 focus:z-10 focus:ring-2 focus:ring-blue-700';
        const activeClasses = 'bg-blue-500 text-white hover:bg-blue-600 active:bg-blue-600 active:text-slate-100';
        const inactiveClasses = 'bg-white text-gray-900 hover:text-blue-700';
        return [
            baseClasses,
            selectedLanguage.value === lang ? activeClasses : inactiveClasses
        ]
    });

    const getActionsClasses = computed(()=>{

        const baseClasses = 'px-4 py-3 text-sm font-medium text-gray-900 bg-white border border-gray-400 rounded-s-lg rounded-lg shadow hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700'
        return [
            baseClasses
        ]
    })

    onMounted(async () => {
        await getTranscriptionById(route.params.id);
        await getInferencesByIdVideoTranscription(route.params.id);
    });
</script>
