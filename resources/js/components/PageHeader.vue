<template>
    <div class="flex flex-wrap items-center justify-between bg-white dark:bg-white/[0.03] px-5 py-3 rounded">
        <div class="flex flex-col items-start justify-start ">
            <h2 class="text-xl font-semibold text-slate-900 dark:text-white/90" x-text="pageTitle">
                {{ title }}
            </h2>
            <nav>
                <ol class="flex items-center gap-1.5">
                    <li v-for="list in breadcrumbs" :key="list">
                        <router-link v-if="list.route"
                            class="inline-flex items-center gap-1.5 text-sm text-gray-600 dark:text-gray-400"
                            :to="list.route">
                            {{ list.name }}
                            <svg class="stroke-current" width="17" height="16" viewBox="0 0 17 16" fill="none"
                                xmlns="http://www.w3.org/2000/svg">
                                <path d="M6.0765 12.667L10.2432 8.50033L6.0765 4.33366" stroke="currentColor"
                                    stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round" />
                            </svg>
                        </router-link>
                        <span v-else class="text-sm text-slate-600 dark:text-white/90">
                            {{ list.name }}
                        </span>
                    </li>
                </ol>
            </nav>
        </div>
        <div class="header-action-section" v-if="showActionBtn && checkPermissions(permission)">
            <small-button v-if="showActionBtn && checkPermissions(permission)" :name="actionBtnName"
                @emitClick="handleClick" customClasses="bg-blue-500 rounded py-1.5" />
        </div>
    </div>
</template>

<script setup>

import { checkPermissions } from '@/utils/helper.js';

const props = defineProps({
    breadcrumbs: {
        type: Array,
        required: true
    },
    title: {
        type: String,
        required: true,
    },
    actionBtnName: {
        type: String,
        default: 'Add New'
    },
    showActionBtn: {
        type: Boolean,
        default: true,
    },
    permission: {
        type: String,
        default: '',
    }
});

const emit = defineEmits(['action'])

const handleClick = () => {
    emit('action')
}
</script>
