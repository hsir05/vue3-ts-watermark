<template>
    <div class="" ref="waterMarkRef" :style="{ height, width }"></div>
</template>
<script lang="ts" setup>
import { ref, toRefs, onMounted } from "vue";
import watermark from 'watermark-dom'
const props = defineProps({
    width: {
        type: String,
        default: "600px",
    },
    height: {
        type: String,
        default: "300px",
    },
})

const { width, height } = toRefs(props)
const waterMarkRef = ref<HTMLDivElement | null>(null);

onMounted(()=> {
watermark.init({ watermark_txt: "测试水印，1021002301，测试水印，100101010111101" });
})
</script>