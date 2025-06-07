<script lang="ts" setup>
    type Props = {
        borderBottom?: boolean
    }
    const props = withDefaults(defineProps<Props>(), {
        borderBottom: false
    });
</script>

<template>
    <ActionsContainer :borderBottom="borderBottom">
        <slot>
        </slot>
    </ActionsContainer>
</template>