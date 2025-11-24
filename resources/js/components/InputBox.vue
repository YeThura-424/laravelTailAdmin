<template>
    <div class="relative w-full">
        <input ref="EditInput" v-model="inputValue" :type="inputType" :placeholder="placeholder" :readonly="readonly"
            class="h-11 w-full rounded-lg border px-4 py-2.5 text-sm shadow-theme-xs focus:outline-none border-gray-200 bg-white text-gray-900 placeholder:text-gray-400 focus:border-gray-300 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30" />
        <div v-if="readonly" @click="EditText"
            class="absolute top-0 right-0 flex h-full w-10 items-center justify-center cursor-pointer">
            <Icon icon="circum:edit" class="text-xl text-gray-500 dark:text-white" />
        </div>
        <div class="show-password absolute right-4 top-1/2 -translate-y-1/2" v-if="type == 'password'">
            <Icon @click="toggleShowPassword" :icon="inputType == 'text' ? 'ion:eye-off-sharp' : 'ion:eye-sharp'"
                size="26" class="text-gray-500 dark:text-white cursor-pointer" />
        </div>
    </div>
</template>

<script setup>
import { watch } from 'vue';
import { ref } from 'vue';


const props = defineProps({
    placeholder: {
        type: String,
        default: "type message here"
    },
    type: {
        type: String,
        default: 'text'
    },
    readonly: {
        type: Boolean,
        default: false
    },
})

const EditInput = ref(null);

const inputValue = defineModel();
const inputType = ref(props.type);
const emits = defineEmits(['edit']);

const EditText = () => {
    emits('edit');
    EditInput.value.focus();
}

const toggleShowPassword = () => {
    inputType.value = inputType.value == "text" ? "password" : "text";
};

watch(inputValue, (val) => {
    if (inputType.value !== 'number') return;
    const current = String(val ?? '');
    const filtered = current.replace(/[^0-9]/g, '');
    if (filtered !== current) {
        inputValue.value = filtered;
    }
});

</script>

<style scoped>
input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

/* Firefox */
input[type="number"] {
    -moz-appearance: textfield;
}
</style>
