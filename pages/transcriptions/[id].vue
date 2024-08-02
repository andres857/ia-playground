<template>
    <section v-if="isLoading" class=" grid bg-violet-400 min-h-screen place-content-center">
        <h3 class="place-content-center"> Loading </h3>
    </section>

    <section v-else class="grid grid-cols-12 min-h-screen bg-slate-200">
        <div class="grid col-span-8 ">
            <div class="col-span-10 h-[100vh]">
                <!-- description transcription -->
                <div class="h-[25vh]">
                    <h1 class=" font-bold">Estado de la transcripción</h1>
                    <div class="grid grid-cols-3 text-md">
                        <p>Cliente MyZoneGo: {{ transcriptionDetails.id_mzg_customer }}</p>
                        <p>Club MyZoneGo: {{ transcriptionDetails.id_mzg_club }}</p>
                        <p>Id Contenido: {{ transcriptionDetails.id_mzg_content }}</p>
                    </div>
                    <div class="grid text-sm mt-2">
                        <p> ID de la Transcripción: {{ transcriptionDetails._id }}</p>
                        <p> Estado: {{ transcriptionDetailsTask.state }}</p>
                        <p> Mensaje: {{ transcriptionDetailsTask.message }}</p> 
                        <p> Fecha de creacion: {{new Date(transcriptionDetails.created_at).toLocaleDateString('en-eu', { weekday:"long", year:"numeric", month:"short", day:"numeric"})}} </p>
                        <p> Ultima actualizacion: {{new Date(transcriptionDetails.updated_at).toLocaleDateString('en-eu', { weekday:"long", year:"numeric", month:"short", day:"numeric"})}}</p>
                        <p></p>
                    </div>
                </div>
                <div class="h-[50vh] flex items-center justify-center">
                    <div class="h-72">
                        <VideoPlayer :urlPlayer="transcriptionDetails.video_url"/>
                    </div>
                </div>
                <div class=" h-[25vh] overflow-hidden">
                   <div class="flex gap-2 items-center justify-center mt-2">
                        
                        <div class=" flex flex-col items-center justify-center p-3 bg-white border border-gray-200 rounded-lg shadow">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6 px-auto">
                                <path stroke-linecap="round" stroke-linejoin="round" d="M7.5 8.25h9m-9 3H12m-9.75 1.51c0 1.6 1.123 2.994 2.707 3.227 1.129.166 2.27.293 3.423.379.35.026.67.21.865.501L12 21l2.755-4.133a1.14 1.14 0 0 1 .865-.501 48.172 48.172 0 0 0 3.423-.379c1.584-.233 2.707-1.626 2.707-3.228V6.741c0-1.602-1.123-2.995-2.707-3.228A48.394 48.394 0 0 0 12 3c-2.392 0-4.744.175-7.043.513C3.373 3.746 2.25 5.14 2.25 6.741v6.018Z" />
                            </svg>

                            <p class="text-sm text-gray-900 pt-1 ">Transcriptor</p>
                        </div>

                        <div class=" flex flex-col items-center justify-center p-3 bg-white border border-gray-200 rounded-lg shadow">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                                <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 14.25v-2.625a3.375 3.375 0 0 0-3.375-3.375h-1.5A1.125 1.125 0 0 1 13.5 7.125v-1.5a3.375 3.375 0 0 0-3.375-3.375H8.25m0 12.75h7.5m-7.5 3H12M10.5 2.25H5.625c-.621 0-1.125.504-1.125 1.125v17.25c0 .621.504 1.125 1.125 1.125h12.75c.621 0 1.125-.504 1.125-1.125V11.25a9 9 0 0 0-9-9Z" />
                            </svg>  
                              
                            <p class="text-sm text-gray-900 pt-1 ">Resumen</p>
                        </div>

                        <div class=" flex flex-col items-center justify-center p-3 bg-white border border-gray-200 rounded-lg shadow">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                                <path stroke-linecap="round" stroke-linejoin="round" d="M9 12h3.75M9 15h3.75M9 18h3.75m3 .75H18a2.25 2.25 0 0 0 2.25-2.25V6.108c0-1.135-.845-2.098-1.976-2.192a48.424 48.424 0 0 0-1.123-.08m-5.801 0c-.065.21-.1.433-.1.664 0 .414.336.75.75.75h4.5a.75.75 0 0 0 .75-.75 2.25 2.25 0 0 0-.1-.664m-5.8 0A2.251 2.251 0 0 1 13.5 2.25H15c1.012 0 1.867.668 2.15 1.586m-5.8 0c-.376.023-.75.05-1.124.08C9.095 4.01 8.25 4.973 8.25 6.108V8.25m0 0H4.875c-.621 0-1.125.504-1.125 1.125v11.25c0 .621.504 1.125 1.125 1.125h9.75c.621 0 1.125-.504 1.125-1.125V9.375c0-.621-.504-1.125-1.125-1.125H8.25ZM6.75 12h.008v.008H6.75V12Zm0 3h.008v.008H6.75V15Zm0 3h.008v.008H6.75V18Z" />
                              </svg>
                              
                            <p class="text-sm text-gray-900 pt-1 ">Ideas clave</p>
                        </div>

                        <div class=" flex flex-col items-center justify-center p-3 bg-white border border-gray-200 rounded-lg shadow">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                                <path stroke-linecap="round" stroke-linejoin="round" d="M8.242 5.992h12m-12 6.003H20.24m-12 5.999h12M4.117 7.495v-3.75H2.99m1.125 3.75H2.99m1.125 0H5.24m-1.92 2.577a1.125 1.125 0 1 1 1.591 1.59l-1.83 1.83h2.16M2.99 15.745h1.125a1.125 1.125 0 0 1 0 2.25H3.74m0-.002h.375a1.125 1.125 0 0 1 0 2.25H2.99" />
                            </svg>
                              
                            <p class="text-sm text-gray-900 pt-1 ">Preguntas</p>
                        </div>
                   </div>
                </div>
            </div>
        </div>
        <!-- Zona de resumenes del video -->
        <div class="grid col-span-4 bg-blue-400">
            Transcriptor
        </div>
    </section>
</template>
  
<script lang="ts" setup>
    import axios from 'axios';
    import { ref, onMounted } from 'vue';
    import { useRoute } from 'vue-router';
    import VideoPlayer from '~/components/VideoPlayer.vue';

    const route = useRoute()
    const apiHost = 'http://192.168.0.102:1905';
    // const apiHost = 'http://190.27.183.156:1905';

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
