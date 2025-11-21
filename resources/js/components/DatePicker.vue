<template>
    <VueDatePicker v-model="dateValue" :placeholder="placeholder" :enable-time-picker="false" dark
        :allowed-dates="allowedDates" auto-apply />
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
    enableTimePicker: {
        type: Boolean,
        default: false
    },
    placeholder: {
        type: String,
        default: 'Pick a date..'
    }

})
const dateValue = defineModel()

const allowedDates = computed(() => {
    const today = new Date();
    today.setHours(0, 0, 0, 0);

    const daysBeforeToday = 30;
    const result = [];

    for (let i = 0; i <= daysBeforeToday; i++) {
        const date = new Date();
        date.setDate(today.getDate() - i);
        result.push(new Date(date));
    }

    return result;
});
</script>

<style>
.dp__input {
    height: 44px !important;
    background: #101828 !important;
    border-color: #344054 !important;
    border-radius: 0.5rem !important;
}

.dp__input:hover {
    border-color: #344054 !important;
}
</style>
