<script lang="ts" setup>
    type Props = {
        styles?: Record<string, string>;
    }
    const props = defineProps<Props>();
</script>

<template>
    <div class="files-wrapper-main" :style="styles">
        <div class="files-wrapper">
            <slot name="upload"></slot>
    
            <section class="files-container">
                <h3>Загруженные файлы</h3>
    
                <div class="files">
                    <slot name="files"></slot>
                </div>
            </section>
        </div>
    </div>
</template>

<style scoped>
    .files-wrapper-main {
        position: relative;
        flex: 1 1 40%;
    }
    .files-wrapper {
        position: absolute;
        top: 0;
        bottom: 0;
        right: 0;
        left: 0;
        padding: 10px 20px;
        border: 1px solid var(--secondary-color);
        border-radius: 15px;
        display: flex;
        flex-direction: column;
        overflow-y: auto;
        gap: 10px;
    }
    .files-wrapper .files-container {
        display: flex;
        flex-direction: column;
        gap: 5px;
    }
    .files-wrapper .files-container h3 {
        font-weight: 700;
    }
    .files-wrapper .files-container .files {
        display: flex;
        flex-direction: column;
    }
</style>