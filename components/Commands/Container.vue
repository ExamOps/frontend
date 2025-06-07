<template>
    <div class="commands-container">
        <slot name="header"></slot>

        <div class="commands">
            <slot name="commands"></slot>
        </div>
    </div>
</template>

<style scoped>
    .commands-container {
        display: flex;
        flex-direction: column;
        border: 1px solid var(--secondary-color);
        border-radius: 15px;
    }
    .commands-container .commands {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        gap: 5px;
        padding: 10px 20px;
    }
</style>