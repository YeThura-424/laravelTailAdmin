<template>
    <div class="text-editor-wrapper">
        <QuillEditor v-model:content="editorValue" contentType="text" toolbar="essential" theme="snow"
            :placeholder="placeholder" />
    </div>
</template>

<script setup>
import { QuillEditor } from '@vueup/vue-quill'
import '@vueup/vue-quill/dist/vue-quill.snow.css';

defineProps({
    placeholder: {
        type: String,
        default: 'Type something...'
    }
})


const editorValue = defineModel()

</script>

<style>
.ql-container {
    min-height: 20dvh;
}
</style>
