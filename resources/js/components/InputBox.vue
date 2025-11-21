<template>
    <div class="w-full">
        <div class="relative w-full">
            <input ref="EditInput" v-model="inputValue" :type="inputType" :placeholder="placeholder"
                :readonly="readonly"
                class="bg-dark-900 h-11 w-full rounded-lg border px-4 py-2.5 text-sm shadow-theme-xs focus:outline-none  border-gray-700 bg-gray-900 text-white/90 placeholder:text-white/30 focus:border-brand-800" />
            <div v-if="readonly" @click="EditText"
                class="absolute top-0 right-0 flex h-full w-10 items-center justify-center cursor-pointer">
                <Icon icon="circum:edit" class="text-xl" />
            </div>
            <div class="show-password absolute right-4 top-1/2 -translate-y-1/2" v-if="type == 'password'">
                <Icon @click="toggleShowPassword" :icon="inputType == 'text' ? 'ion:eye-off-sharp' : 'ion:eye-sharp'"
                    size="26" class="text-white cursor-pointer" />
            </div>
        </div>
    </div>
</template>

<script setup>
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
</script>
