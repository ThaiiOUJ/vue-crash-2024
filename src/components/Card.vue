<template>
     <div :class="`${bg} p-6 rounded-lg shadow-md`">
        <slot>
        </slot>
     </div>
</template>

<script setup>
import {defineProps} from'vue';

defineProps({
    // 用bg来充当背景缩写
    bg:{
        type:String,
        default:'bg-gray-100'
    }
})
</script>