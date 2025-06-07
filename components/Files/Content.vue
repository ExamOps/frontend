<script lang="ts" setup>
    type Props = {
        files: Array<any>;
        styles?: Record<string, string>;
        isDelete?: boolean;
    };
    const props = withDefaults(defineProps<Props>(), {
        isDelete: false
    });
</script>

<template>
    <FilesContainer :styles="styles">
        <template #upload>
            <slot>
            </slot>
        </template>

        <template #files>
            <FilesFile
                v-for="(file, index) in files"
                :key="file"
                :title="file.title"
                :type="file.type"
                :weight="file.weight"
                :is-last="index == files.length - 1"
                :is-delete="isDelete"
            />
        </template>
    </FilesContainer>
</template>