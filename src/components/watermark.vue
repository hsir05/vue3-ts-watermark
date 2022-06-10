<template>
    <div class="" ref="waterMarkRef" :style="{ height, width }"></div>
</template>
<script setup lang="ts">
import { Ref, ref, onMounted, unref, toRefs } from 'vue'

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

const eleId = "waterMarkId"

interface WaterMarkInter {
    elRef: Ref<HTMLDivElement>;
    waterMark: string; 
    color?:string; 
    angle?: number; 
    width?: number;  
    height?: number; 
    alpha?: number; 
    size?: string;
}


function addWaterMarker({elRef, waterMark="水印", color = "#B8BECC", angle=-15, size="24", alpha=0.5, width=600, height=300}: WaterMarkInter) {
    if (!elRef) {
        return
    }
    
    const element = unref(elRef) as HTMLDivElement
    let canvasEle = document.createElement('canvas');
    element.appendChild(canvasEle);
    canvasEle.id = eleId
    canvasEle.width = width;
    canvasEle.height = height;
    canvasEle.style.display = 'none';
    canvasEle.style.zIndex = '9'
    let cans = canvasEle.getContext('2d') as CanvasRenderingContext2D;
    cans.rotate(angle * Math.PI / 180);
    cans.font = "500 30px Microsoft JhengHei";
    cans.fillStyle = color;
    cans.textAlign = 'center';
    cans.textBaseline = 'middle';
    cans.globalAlpha = alpha
    cans.font = '1000 ' + size + 'px ' + ' Microsoft JhengHei'
    for (let i = (document.body.offsetHeight * 0.5) * -1; i < 800; i += 160) {
        for (let j = 0; j < document.body.offsetHeight * 1.5; j += 60) {
            cans.fillText(waterMark, i, j)
        }
    }
    element.style.backgroundImage = "url(" + canvasEle.toDataURL("image/png") + ")";
}

//  function removeWatermark (eleId: string){
//   if (document.getElementById(eleId) !== null) {
//     document.body.removeChild(document.getElementById(eleId) as HTMLDivElement)
//   }
//  }


onMounted(() => {
    addWaterMarker({
        elRef: waterMarkRef as Ref<HTMLDivElement>, 
        waterMark: "测试",
        size: "28",
        color: 'skyblue'
    })

})
</script>
