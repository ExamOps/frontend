<script setup lang="ts">
    type Props = {
        item: any;
        call: any;
        api: any;
    }
    const props = defineProps<Props>();
    const activeCup = defineModel<string>();
    const emit = defineEmits(['mouseenter', 'mouseleave']);
</script>

<template>
    <div 
        class="data-row"
        @click="activeCup = item.cup" 
        @mouseleave="emit('mouseleave')"
        @mouseenter="emit('mouseenter')"
    >
        <slot name="columns"></slot>
    </div>

    <slot name="expanded"></slot>
</template>

<style scoped>
    .data-row {
        display: contents;
    }
    .data-row.active {
        background: var(--secondary-color);
    }
</style>