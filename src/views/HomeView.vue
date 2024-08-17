<script setup lang="ts">
import { defineComponent, onMounted, ref, toRaw } from 'vue';
import MultiStep from '@/components/MultiStep.vue'; // @ is an alias to /src
import { StepForm } from '@/types/types';

const data = ref<StepForm[]>([]);

const fetchData = async () => {
  const newData = await fetch('response.json');
  const result = await newData.json();
  data.value = result;
};

onMounted(() => {
  fetchData();
});
</script>

<template>
  <MultiStep :data="data" />
</template>
