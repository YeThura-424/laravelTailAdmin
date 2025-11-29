<template>
    <div class="user-form-wrapper bg-white dark:bg-white/[0.03] px-2 rounded text-gray-900 dark:text-white">
        <div class="grid grid-cols-5 gap-x-5 py-2.5">
            <div class="col-span-2 rounded-md px-3 py-0.5">
                <div class="user-profile-section w-full h-full flex flex-col gap-y-2 py-2">
                    <label class="text-sm text-nowrap">User
                        Profile</label>
                    <file-upload v-model="form.profile" />
                </div>
            </div>
            <div class="col-span-3 rounded-md px-3 py-0.5">
                <div class="user-name flex flex-col gap-y-2 py-2">
                    <label class="text-sm text-nowrap">User
                        Name</label>
                    <input-box v-model="form.name" placeholder="John Doe" type="text" />
                </div>
                <div class="user-email flex flex-col gap-y-2 py-2">
                    <label class="text-sm text-nowrap">Email</label>
                    <input-box v-model="form.email" placeholder="johndoe@gmail.com" type="email" />
                </div>
                <div class="user-phone flex flex-col gap-y-2 py-2">
                    <label class="text-sm text-nowrap">Phone</label>
                    <input-box v-model="form.phone" placeholder="09987654321" type="tel" />
                </div>
            </div>
        </div>
        <div class="grid grid-cols-4 gap-x-5 py-2.5">
            <div class="col-span-2 rounded-md px-3 py-0.5">
                <div class="user-name flex flex-col gap-y-2 py-2">
                    <label class="text-sm text-nowrap">Gender</label>
                    <input-box v-model="form.gender" placeholder="Male" type="text" />
                </div>
                <div class="user-email flex flex-col gap-y-2 py-2">
                    <label class="text-sm text-nowrap">Address</label>
                    <textarea-box v-model="form.address" placeholder="No.13, SeintJ Street, ..." :rows="4" />
                </div>
            </div>
            <div class="col-span-2 rounded-md px-3 py-0.5">
                <div class="user-phone flex flex-col gap-y-2 py-2">
                    <label class="text-sm text-nowrap">Nrc
                        No</label>
                    <nrc-selector v-model="form.nrc_no" />
                </div>
                <div class="user-name flex flex-col gap-y-2 py-2">
                    <label class="text-sm text-nowrap">Education</label>
                    <input-box v-model="form.education" placeholder="Under graduate" type="text" />
                </div>
                <div class="user-email flex flex-col gap-y-2 py-2">
                    <label class="text-sm text-nowrap">Password</label>
                    <input-box v-model="form.password" placeholder="*********" type="password" />
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { reactive, watch } from 'vue';

const props = defineProps({
    modelValue: {
        type: Object,
        default: () => { },
    },
})


const form = reactive({
    name: '',
    email: '',
    phone: '',
    profile: '',
    gender: '',
    address: '',
    nrc_no: '',
    education: '',
    password: ''
});

watch(() => props.modelValue, (updateVal) => {
    form.name = updateVal?.name;
    form.email = updateVal?.email;
    form.phone = updateVal?.phone;
    form.gender = updateVal?.gender;
    form.profile = updateVal?.profile;
    form.address = updateVal?.address;
    form.nrc_no = updateVal?.nrc_no;
    form.education = updateVal?.education;
    form.password = updateVal?.password
})

const emit = defineEmits(["update:modelValue"]);

watch(() => form, (updateForm) => {
    emit('update:modelValue', updateForm)
}, { deep: true })

</script>
