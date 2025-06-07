<script setup lang="ts">
    const props = defineProps<{
        value: string | number;
    }>();
    const emit = defineEmits(['click']);
    const isHover = shallowRef(false);

    const translateKey: any = {
        'time': 'Время',
        'service': 'Сервис',
        'log_type': 'Тип лога',
        'order': 'Порядок',
        'action': 'Действие',
        'limit': 'Лимит',
        'offset': 'Сдвиг',
        'asc': 'По возрастанию',
        'desc': 'По убыванию',
        'day': 'День',
        'weak': 'Неделя',
        'month': 'Месяц',
        'year': 'Год'
    }

    const displayValue = computed(() => {
        return typeof props.value === 'string' && translateKey[props.value.toLowerCase()] 
            ? translateKey[props.value.toLowerCase()] 
            : props.value;
    });
</script>

<template>
    <button 
        class="active-filter"
        @click="emit('click', value)"
        @mouseover="isHover = true"
        @mouseleave="isHover = false"
    >
        <span>
            {{ displayValue }}
        </span>

        <transition mode="default" name="fade">
            <div v-if="isHover" class="active-filter__close">
                <v-icon icon="mdi-close"/>
            </div>
        </transition>
    </button>
</template>

<style scoped>
    .active-filter {
        position: relative;
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: 5px;
        min-width: 60px;
        padding: 4px 10px;
        border: 2px solid var(--active-color);
        border-radius: 12px;
        transition: all 0.3s linear;
    }
    .active-filter:hover {
        border-color: var(--delete-color);
    }
    .active-filter span {
        flex: 1 1 auto;
        display: flex;
        margin-right: 20px;
    }
    .active-filter__close {
        position: absolute;
        top: 6px;
        right: 6px;
        color: var(--delete-color);
        font-size: 12px;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
    }

    .fade-enter-active,
    .fade-leave-active {
        transition: opacity .3s linear;
    }

    .fade-enter,
    .fade-leave-to {
        opacity: 0
    }
</style>