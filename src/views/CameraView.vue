<script setup lang="ts">
import { ref, onMounted } from "vue";
const video = ref<HTMLVideoElement | null>(null);
const canvas = ref<HTMLCanvasElement | null>(null);
const ctx = ref<any>(null);

const constraints = ref({
    audio: false,
    video: {
        facingMode: 'environment'
    }
});
onMounted(async () => {
    if (video.value) {
        ctx.value = canvas.value?.getContext('2d');
        await navigator
            .mediaDevices
            .getUserMedia(constraints.value)
            .then(setStream)
            .catch(e => console.error(e));
    }
});
const setStream = (stream: any) => {
    if (!video.value) return;
    video.value.srcObject = stream;
    video.value.play();
    requestAnimationFrame(draw);
}
const draw = () => {
    ctx.value.drawImage(video.value, 0, 0, canvas.value?.width, canvas.value?.height);
    requestAnimationFrame(draw);
}


</script>

<template>

    <video ref="video" autoplay playsinline webkit-playsinline muted hidden></video>
    <canvas ref="canvas" width="1080" height="1920" class="canvas"></canvas>
</template>

<style>
.canvas {
    background-color: black;
    border-radius: 50px;
}
</style>