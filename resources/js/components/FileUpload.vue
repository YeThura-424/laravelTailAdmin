<template>
    <div :class="[
        'upload-wrapper flex p-12 border border-[#e1def51f] rounded relative',
        previews.length > 0 ? ' flex-wrap justify-start items-center gap-3' : 'flex-col justify-center items-center gap-y-3',
    ]">
        <!-- File input -->
        <div class="input-container absolute inset-0">
            <input id="file-upload" multiple type="file" class="absolute opacity-0 w-full h-full cursor-pointer"
                @change="handleUpload" />
        </div>

        <!-- Upload prompt -->
        <template v-if="previews.length === 0">
            <div class="upload-icon bg-[#43465d] p-2 rounded">
                <Icon icon="lucide:upload" class="text-[#e1def5e6] text-xl" />
            </div>
            <div class="upload-message text-center">
                <p class="text-[#e1def5e6] text-2xl">Drag and drop your image here.</p>
                <p class="text-[#e1def566] text-sm">or</p>
            </div>
            <div
                class="upload-button z-2 relative bg-[#393b63] rounded transition duration-300 px-4 py-1 text-[#e1def5e6] hover:bg-[#474a77]">
                Browse Images
                <div class="input-container absolute top-0 left-0 right-0 bottom-0">
                    <input id="file-upload" multiple type="file" class="opacity-0 w-full h-full cursor-pointer"
                        @change="handleUpload" />
                </div>
            </div>
        </template>

        <!-- Previews -->
        <div v-for="(item, index) in previews" :key="item.url"
            class="image-preview-section bg-[#2f3349] rounded p-6 look-box-shadow flex flex-col justify-center items-center gap-y-3">
            <div class="image-container">
                <img :src="item.url" alt="" class="w-32 h-32 rounded-lg p-1 object-cover" />
            </div>
            <div class="remove-button z-2">
                <button @click="handleRemove(index)"
                    class="bg-[#393b63] rounded transition duration-300 px-4 py-1 text-[#e1def5e6] hover:bg-[#474a77]">
                    Remove File
                </button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
    modelValue: { type: Array, default: () => [] },
    multiple: { type: Boolean, default: false },
    showPreview: { type: Boolean, default: true },
});
const emit = defineEmits(['update:modelValue']);

const previews = ref([]);

// Sync incoming modelValue if it's already previewable
watch(
    () => props.modelValue,
    (val) => {
        if (Array.isArray(val)) {
            previews.value = val.map((file) =>
                typeof file === 'object'
                    ? file
                    : { url: file, file: null }
            );
        }
    },
    { immediate: true }
);

const handleUpload = (e) => {
    const files = Array.from(e.target.files);

    const newPreviews = files.map((file) => ({
        url: URL.createObjectURL(file),
        file,
    }));

    previews.value.push(...newPreviews);

    emit(
        'update:modelValue',
        previews.value.map((p) => p.file)
    );
};

const handleRemove = (index) => {
    previews.value.splice(index, 1);
    emit(
        'update:modelValue',
        previews.value.map((p) => p.file)
    );
};
</script>

<style scoped>
input#file-upload button {
    display: none;
}
</style>

<style>
.look-box-shadow {
    box-shadow: 0 3px 12px rgba(var(--v-shadow-key-umbra-color), var(--v-shadow-md-opacity)), 0 0 transparent,
        0 0 transparent;
}
</style>
