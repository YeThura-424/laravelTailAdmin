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
.ql-editor.ql-blank::before {
    color: white;
}

.ql-toolbar.ql-snow {
    border-radius: 5px 5px 0 0;
    border-color: #e1def51f;
}

.ql-toolbar.ql-snow+.ql-container.ql-snow {
    border-radius: 0 0 5px 5px;
    border-color: #e1def51f;
}

.ql-container {
    min-height: 20dvh;
}
</style>
