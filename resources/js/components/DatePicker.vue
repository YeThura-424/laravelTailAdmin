<template>
    <div :class="['so-datepicker', isDark ? 'so-datepicker--dark' : 'so-datepicker--light']">
        <VueDatePicker v-model="dateValue" :placeholder="placeholder" :enable-time-picker="enableTimePicker"
            :dark="isDark" :allowed-dates="allowedDates" auto-apply />
    </div>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue';

const props = defineProps({
    enableTimePicker: {
        type: Boolean,
        default: false
    },
    placeholder: {
        type: String,
        default: 'Pick a date..'
    },
    theme: {
        type: String,
        default: 'auto' // 'auto' | 'dark' | 'light'
    }

})
const dateValue = defineModel()

const rootHasDark = ref(false);
const mediaDark = ref(false);

let _mo = null;
let _mq = null;
let _mqHandler = null;

const isDark = computed(() => {
    if (props.theme === 'dark') return true;
    if (props.theme === 'light') return false;
    return rootHasDark.value || mediaDark.value;
});

onMounted(() => {
    if (typeof window === 'undefined') return;

    // initial values
    rootHasDark.value = document.documentElement.classList.contains('dark');

    // observe changes to root element's class attribute
    _mo = new MutationObserver((mutations) => {
        for (const m of mutations) {
            if (m.type === 'attributes' && m.attributeName === 'class') {
                rootHasDark.value = document.documentElement.classList.contains('dark');
            }
        }
    });
    _mo.observe(document.documentElement, { attributes: true, attributeFilter: ['class'] });

    // listen to prefers-color-scheme changes
    if (window.matchMedia) {
        _mq = window.matchMedia('(prefers-color-scheme: dark)');
        mediaDark.value = _mq.matches;
        _mqHandler = (e) => { mediaDark.value = e.matches; };
        if (typeof _mq.addEventListener === 'function') {
            _mq.addEventListener('change', _mqHandler);
        } else if (typeof _mq.addListener === 'function') {
            _mq.addListener(_mqHandler);
        }
    }
});

onBeforeUnmount(() => {
    if (_mo) _mo.disconnect();
    if (_mq && _mqHandler) {
        if (typeof _mq.removeEventListener === 'function') {
            _mq.removeEventListener('change', _mqHandler);
        } else if (typeof _mq.removeListener === 'function') {
            _mq.removeListener(_mqHandler);
        }
    }
});

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
.so-datepicker--dark .dp__input {
    height: 44px !important;
    background: #101828 !important;
    border-color: #344054 !important;
    border-radius: 0.5rem !important;
    color: #f8fafc !important;
}

.so-datepicker--dark .dp__input:hover {
    border-color: #344054 !important;
}

.so-datepicker--light .dp__input {
    height: 44px !important;
    background: #ffffff !important;
    border-color: #e5e7eb !important;
    border-radius: 0.5rem !important;
    color: #0f172a !important;
}

.so-datepicker--light .dp__input:hover {
    border-color: #cbd5e1 !important;
}
</style>
