<script setup lang="ts">
import { ref, watch } from 'vue';
import { StepForm } from '@/types/types';
import Form from './Form.vue';
import PreviewForm from './PreviewForm.vue';

const props = defineProps<{
  data: StepForm[];
}>();

const currentStep = ref<number>(1);
const formData = ref<Record<string, any>>({});
const isPreview = ref<boolean>(false);

const nextStep = () => {
  if (currentStep.value < props.data.length) {
    currentStep.value++;
  }
};
const prevStep = () => {
  if (currentStep.value > 1) {
    currentStep.value--;
  }
};

const onSubmit = () => {
  console.log(formData.value);
  isPreview.value = true;
};

const reSubmit = () => {
  isPreview.value = false;
  currentStep.value = 1;
  formData.value = {};
};
</script>

<template>
  <div v-if="!isPreview">
    <div class="w-[50vw] mx-auto p-6 bg-slate-300 rounded-xl">
      <div class="flex justify-center items-center gap-8">
        <!-- Step -->
        <div v-for="(page, index) in data" :key="index">
          <!-- Previous -->
          <div
            v-if="index + 1 < currentStep"
            class="text-white text-3xl size-14 bg-gray-400 flex justify-center items-center rounded-full"
            >{{ index + 1 }}</div
          >

          <!-- Current -->
          <div
            v-if="index + 1 === currentStep"
            class="text-white text-3xl size-14 bg-green-600 flex justify-center items-center rounded-full"
            >{{ index + 1 }}</div
          >
          <!-- Next -->
          <div
            v-if="index + 1 > currentStep"
            class="text-white text-3xl size-14 bg-gray-400 flex justify-center items-center rounded-full"
            >{{ index + 1 }}</div
          >
        </div>
        <!-- Step -->
      </div>
      <h1 class="text-xl font-bold">
        {{ props.data[currentStep - 1]?.title }}
      </h1>
      <p>{{ props.data[currentStep - 1]?.description }}</p>
      <div v-if="props.data">
        <Form
          :formFields="props.data[currentStep - 1]?.fields"
          :value="formData.value"
          @update:formValue="(newData:any) => (formData.value = newData)"
          :isPreview="false"
        />
      </div>
      <div class="flex justify-between">
        <div class="flex gap-4">
          <!-- Button -->
          <button
            @click.prevent="prevStep"
            :disabled="currentStep == 1"
            class="shadow bg-green-700 hover:bg-green-600 focus:shadow-outline disabled:cursor-not-allowed disabled:hover:bg-gray-400 disabled:bg-gray-400 focus:outline-none text-white font-bold py-2 px-4 rounded cursor-pointer"
            >Previous</button
          >
          <button
            @click.prevent="nextStep"
            :disabled="currentStep == props.data.length"
            class="shadow bg-green-600 hover:bg-green-500 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded disabled:cursor-not-allowed disabled:hover:bg-gray-400 disabled:bg-gray-400"
            >Next</button
          >
        </div>
        <!-- Button -->

        <!-- Submit Button -->
        <div v-if="currentStep == props.data.length">
          <button
            @click.prevent="onSubmit"
            class="shadow bg-green-600 hover:bg-green-500 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded disabled:cursor-not-allowed disabled:hover:bg-gray-400 disabled:bg-gray-400"
            >Submit</button
          >
        </div>
      </div>
    </div>
  </div>
  <div v-if="isPreview">
    <PreviewForm
      :submittedForm="data"
      :data="formData.value"
      :reSubmit="reSubmit"
    />
  </div>
</template>
