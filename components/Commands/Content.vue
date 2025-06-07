<script lang="ts" setup>
    type Props = {
        commands: Array<string>
    };
    const props = defineProps<Props>();
    const emit = defineEmits(['click']);
</script>

<template>
    <CommandsContainer>
        <template #header>
            <CommandsHeader>
                <slot></slot>
            </CommandsHeader>
        </template>

        <template #commands>
            <CommandsCommand
                v-for="command in commands"
                :key="command"
                :value="command"
                @click="(value) => emit('click', value)"
            />
        </template>
    </CommandsContainer>
</template>