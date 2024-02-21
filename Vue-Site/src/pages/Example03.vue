<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';

interface MediaResponse {
    title: string;
    url: string;
    explanation: string;
    media_type: string;
}

export default defineComponent({
    setup() {
        const dayImage = ref<MediaResponse>({
            title: '',
            url: '',
            explanation: '',
            media_type: ''
        });

        const getMedia = async () => {
            try {
                const res = await fetch('https://api.nasa.gov/planetary/apod?api_key=LLWiwai8ZooMBWMdLnNN1nUlLBjtdNRUe9wjH7pA');
                const data: MediaResponse = await res.json();
                dayImage.value = data;
            } catch (error) {
                console.error('Error al obtener el medio desde la API de la NASA:', error);
            }
        };

        onMounted(() => {
            getMedia();
        });

        return {
            dayImage
        };
    },
    computed: {
        mediaType(): string {
            return this.dayImage.media_type;
        }
    }
});
</script>

  
<template>
    <div class="mb-32">
        <h3 class="text-center bg-gray-700 p-4 rounded-2xl m-2 text-3xl">{{ dayImage.title }}</h3>
        <div v-if="mediaType === 'image'">
            <img class="my-6" :src="dayImage.url" alt="Nasa Day Image" />
        </div>
        <div v-else-if="mediaType === 'video'">
            <a href={{ dayImage.url }}>The image of the day is a video, see this!</a>
        </div>
        <div v-else>
            <p>Media type is not valid.</p>
        </div>
        <p class="text-2xl">{{ dayImage.explanation }}</p>
    </div>
</template>