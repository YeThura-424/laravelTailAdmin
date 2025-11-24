<template>
    <div class="relative w-full">
        <textarea v-model="inputValue" :rows="rows" :placeholder="placeholder" :readonly="readonly" ref="EditInput"
            class="w-full rounded-lg border px-4 py-2.5 text-sm shadow-theme-xs focus:outline-none border-gray-200 bg-white text-gray-900 placeholder:text-gray-400 focus:border-gray-300 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30" />
        <div v-if="readonly" @click="EditText" class="absolute top-3 right-0 h-full w-10 cursor-pointer">
            <Icon icon="circum:edit" class="text-xl" />
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
defineProps({
    placeholder: {
        type: String,
        default: "type message here"
    },
    rows: {
        type: Number,
        default: 3
    },
    readonly: {
        type: Boolean,
        default: false
    }
})

const inputValue = defineModel();
const emits = defineEmits(['edit']);
const EditInput = ref(null);
const EditText = () => {
    emits('edit');
    EditInput.value.focus();
}
</script>
