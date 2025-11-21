<template>
    <div class="text-white">
        <!-- NRC Form (Compact Row) -->
        <div class="flex gap-2.5 items-center relative">
            <!-- State -->
            <select v-model="selectedState"
                class="bg-dark-900 h-11 rounded-lg border px-4 py-2.5 text-sm shadow-theme-xs focus:outline-none  border-gray-700 bg-gray-900 text-white/90 placeholder:text-white/30 focus:border-brand-800 w-18">
                <option v-for="state in nrcData.states" :key="state.id" :value="state">
                    {{ state.code_mm }}
                </option>
            </select>

            <!-- Township -->
            <select v-model="selectedTownship"
                class="bg-dark-900 h-11  rounded-lg border px-4 py-2.5 text-sm shadow-theme-xs focus:outline-none  border-gray-700 bg-gray-900 text-white/90 placeholder:text-white/30 focus:border-brand-800 w-26">
                <option v-for="township in selectedState?.townships || []" :key="township.id" :value="township">
                    {{ township.code_mm }}
                </option>
            </select>

            <!-- NRC Type -->
            <select v-model="selectedType"
                class="bg-dark-900 h-11  rounded-lg border px-4 py-2.5 text-sm shadow-theme-xs focus:outline-none  border-gray-700 bg-gray-900 text-white/90 placeholder:text-white/30 focus:border-brand-800 w-24">
                <option v-for="type in nrcData.types" :key="type.id" :value="type">
                    {{ type.code_mm }}
                </option>
            </select>

            <!-- NRC Number -->
            <div>
                <InputBox v-model="nrcNumber" maxlength="6" placeholder="၁၂၃၄၅၆" />
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, watch } from "vue";
import nrcData from "@/dummy/nrc.json";

const props = defineProps({
    modelValue: {
        type: String,
        default: "",
    },
});
const emit = defineEmits(["update:modelValue"]);

const selectedState = ref(null);
const selectedTownship = ref(null);
const selectedType = ref(null);
const nrcNumber = ref("");

// Parse NRC string into parts and update refs
function parseFullNRC(full) {
    const regex = /^([၀-၉0-9]{1,2})\/(.+?)\((.+?)\)([၀-၉0-9]{6})$/u;
    const match = full.match(regex);
    if (!match) return false;

    const [, stateCode, townshipCode, typeCode, number] = match;

    const state = nrcData.states.find((s) => s.code_mm === stateCode) || null;
    if (!state) return false;

    const township =
        state.townships?.find((t) => t.code_mm === townshipCode) || null;
    const type = nrcData.types.find((t) => t.code_mm === typeCode) || null;
    if (!type) return false;

    selectedState.value = state;
    selectedTownship.value = township;
    selectedType.value = type;
    nrcNumber.value = number;

    return true;
}

// Initialize on mount or when modelValue changes
watch(
    () => props.modelValue,
    (newVal) => {
        if (!newVal) {
            // Reset if empty
            selectedState.value = nrcData.states[0] || null;
            selectedTownship.value =
                selectedState.value?.townships?.[0] || null;
            selectedType.value = nrcData.types[0] || null;
            nrcNumber.value = "";
            return;
        }
        parseFullNRC(newVal);
    },
    { immediate: true }
);

// Update township if not matching current when state changes
watch(selectedState, (newState) => {
    if (
        !newState?.townships?.find((t) => t.id === selectedTownship.value?.id)
    ) {
        selectedTownship.value = newState?.townships?.[0] || null;
    }
});

// When any part changes, emit the full NRC string if valid
watch([selectedState, selectedTownship, selectedType, nrcNumber], () => {
    if (
        selectedState.value &&
        selectedTownship.value &&
        selectedType.value &&
        nrcNumber.value.length === 6
    ) {
        const full = `${selectedState.value.code_mm}/${selectedTownship.value.code_mm}(${selectedType.value.code_mm})${nrcNumber.value}`;
        if (full !== props.modelValue) {
            emit("update:modelValue", full);
        }
    }
});
</script>
