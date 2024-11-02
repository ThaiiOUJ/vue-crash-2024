<script>
import jobData from '@/jobs.json';
import { ref } from 'vue';

const jobs = ref(jobData); // 使用 ref 定义响应式数据
console.log(jobs.value); // 输出以检查数据是否加载成功
</script>
<template>
    <section class="bg-blue-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb text-center">
                Browse jobs
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">

            </div>
        </div>
    </section>

    <div v-for="job in jobs" :key="job.id">
                    {{ job.title }}
                </div>
</template>
