<template>
    <TransitionRoot as="template" :show="openModel">
        <Dialog class="relative z-40" @close="handleClose">
            <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0" enter-to="opacity-100"
                leave="ease-in duration-200" leave-from="opacity-100" leave-to="opacity-0">
                <div class="fixed inset-0 bg-gray-900/10 backdrop-blur-[5px] transition-opacity" />
            </TransitionChild>

            <div class="fixed inset-0 z-10 w-screen overflow-y-auto ">
                <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
                    <TransitionChild as="template" enter="ease-out duration-300"
                        enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
                        enter-to="opacity-100 translate-y-0 sm:scale-100" leave="ease-in duration-200"
                        leave-from="opacity-100 translate-y-0 sm:scale-100"
                        leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
                        <DialogPanel
                            class="relative transform  rounded-lg text-left shadow-xl transition-all sm:my-8 min-w-[500px]">
                            <div class="model-body px-3 py-2 bg-[#192841]">
                                <div :class="modelBodyClass">
                                    <div class="mt-3 text-left">
                                        <DialogTitle as="h3" class="text-base font-semibold text-gray-100">{{ title }}
                                        </DialogTitle>
                                        <slot />
                                    </div>
                                </div>
                            </div>
                            <div class="flex justify-end gap-x-4 px-3 pb-4  bg-[#192841]">
                                <button type="button"
                                    class="inline-flex w-full justify-center rounded-md bg-slate-500 px-3 py-2 text-sm font-semibold text-white shadow-xs  sm:w-auto"
                                    @click="handleClose" ref="cancelButtonRef">Cancel</button>
                                <button type="button"
                                    class="inline-flex w-full justify-center rounded-md bg-blue-500 px-3 py-2 text-sm font-semibold text-white shadow-xs sm:w-auto"
                                    @click="handleSubmit" ref="cancelButtonRef">Submit</button>
                            </div>
                        </DialogPanel>
                    </TransitionChild>
                </div>
            </div>
        </Dialog>
    </TransitionRoot>
</template>

<script setup>
import { ref, watch } from 'vue'
import { Dialog, DialogPanel, DialogTitle, TransitionChild, TransitionRoot } from '@headlessui/vue'

const props = defineProps({
    visible: {
        type: Boolean,
        default: false
    },
    title: {
        type: String,
        default: "Add New"
    },
    modelBodyClass: {
        type: String,
        required: false,
    }
})

const emit = defineEmits(['dismiss', 'submit'])

const openModel = ref(false)

watch(() => props.visible, (newVal) => {
    openModel.value = newVal
})

const handleClose = () => {
    openModel.value = false;
    emit('dismiss', false)
}

const handleSubmit = () => {
    emit('submit')
}

</script>
