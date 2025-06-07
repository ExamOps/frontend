<template>
    <div class="header">
        <h3>Разрешенные команды</h3>

        <div class="controls">
            <slot></slot>
        </div>
    </div>
</template>

<style scoped>
    .header {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        padding: 10px 20px;
        border-bottom: 1px solid var(--secondary-color);
    }
    .header h3 {
        font-weight: 700;
    }
</style>