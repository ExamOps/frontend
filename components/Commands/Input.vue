<script lang="ts" setup>
    const value = ref('');
    const emit = defineEmits(['add']);
</script>

<template>
    <div class="field-container">
        <input 
            class="input"
            type="text"
            v-model="value"
        >

        <button @click="emit('add', value)">
            <v-icon>
                mdi-plus
            </v-icon>
        </button>
    </div>
</template>

<style scoped>
    .field-container {
        display: flex;
        flex-direction: row;
        border: 2px solid var(--secondary-color);
        border-radius: 8px;
    }
    .field-container input {
        flex: 1;
        outline: none;
        font-size: 12px;
        padding: 3px 10px;
    }
    .field-container button {
        display: flex;
        justify-content: center;
        padding: 0 2px;
        align-items: center;
        border-left: 2px solid var(--secondary-color);
    }
    .field-container button .v-icon {
        font-size: 18px;
    }
</style>