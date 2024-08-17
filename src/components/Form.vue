<script setup lang="ts">
import { ref, toRefs, watch } from 'vue';
import { Field, StepForm } from '../types/types';

const props = defineProps<{
  formFields: Field[];
  value: Record<string, any>;
  data: Record<string, any>[];
  isPreview: boolean;
}>();

const { formFields, value } = toRefs(props);
const formValue = ref({ ...value.value });

const emit = defineEmits(['update:formValue', 'validation']);

watch(
  formValue,
  (newValue) => {
    emit('update:formValue', newValue);
  },
  { deep: true },
);
</script>

<template>
  <div class="flex flex-col gap-4 my-6">
    <div v-if="!isPreview">
      <div
        v-for="(field, index) in props.formFields"
        :key="field.label"
        class="flex flex-col gap-2"
      >
        <label :for="field.label"
          >{{ field.label }}
          <span v-if="field.required" class="text-red-500">*</span>
        </label>
        <!-- Text -->
        <div v-if="field.type === 'textfield'">
          <input
            type="text"
            :placeholder="field.placeholder"
            v-model="formValue[field.label]"
            class="border p-2 w-full shadow appearance-none rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          />
        </div>

        <!-- Textarea -->
        <div v-if="field.type === 'textarea'">
          <textarea
            :placeholder="field.placeholder"
            v-model="formValue[field.label]"
            class="border p-2 w-full shadow appearance-none rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          ></textarea>
        </div>

        <!-- Radio -->
        <div v-if="field.type === 'radio'">
          <div
            v-for="option in field.options"
            :key="typeof option === 'object' ? option.value : option"
            class="flex items-center"
          >
            <input
              type="radio"
              :value="typeof option === 'object' ? option.value : option"
              v-model="formValue[field.label]"
              class="mr-2"
            />
            <label
              :for="
                typeof option === 'object'
                  ? option.value.toString()
                  : option.toString()
              "
              >{{ typeof option === 'object' ? option.value : option }}</label
            >
          </div>
        </div>

        <!-- AutoComplete -->
        <div v-if="field.type === 'autocomplete'">
          <input
            type="text"
            list="autocompleteOptions"
            :placeholder="field.placeholder"
            v-model="formValue[field.label]"
            class="border p-2 w-full shadow appearance-none rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          />
          <datalist id="autocompleteOptions">
            <option
              v-for="option in field.options"
              :key="typeof option === 'object' ? option.value : option"
            >
              {{ option }}
            </option>
          </datalist>
        </div>
      </div>
    </div>
    <!-- Preview Data -->
    <div v-if="isPreview">
      <div
        v-for="(field, index) in props.formFields"
        :key="field.label"
        class="flex flex-col gap-2"
      >
        <label :for="field.label"
          >{{ field.label }}
          <span v-if="field.required" class="text-red-500">*</span>
        </label>
        <!-- Text -->
        <div v-if="field.type === 'textfield'">
          <input
            type="text"
            :placeholder="field.placeholder"
            :value="data[field.label]"
            disabled
            class="border p-2 w-full shadow appearance-none rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          />
        </div>

        <!-- Textarea -->
        <div v-if="field.type === 'textarea'">
          <textarea
            :placeholder="field.placeholder"
            :value="data[field.label]"
            disabled
            class="border p-2 w-full shadow appearance-none rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          ></textarea>
        </div>

        <!-- Radio -->
        <div v-if="field.type === 'radio'">
          <div
            v-for="option in field.options"
            :key="typeof option === 'object' ? option.value : option"
            class="flex items-center"
          >
            <input
              type="radio"
              :value="typeof option === 'object' ? option.value : option"
              v-model="data[field.label]"
              disabled
              class="mr-2"
            />
            <label
              :for="
                typeof option === 'object'
                  ? option.value.toString()
                  : option.toString()
              "
              >{{ typeof option === 'object' ? option.value : option }}</label
            >
          </div>
        </div>

        <!-- AutoComplete -->
        <div v-if="field.type === 'autocomplete'">
          <input
            type="text"
            list="autocompleteOptions"
            :placeholder="field.placeholder"
            v-model="data[field.label]"
            disabled
            class="border p-2 w-full shadow appearance-none rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          />
          <datalist id="autocompleteOptions">
            <option
              v-for="option in field.options"
              :key="typeof option === 'object' ? option.value : option"
            >
              {{ option }}
            </option>
          </datalist>
        </div>
      </div>
    </div>
  </div>
</template>
