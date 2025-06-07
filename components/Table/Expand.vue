<template>
    <div class="data-row">
        <div class="container">
            <slot></slot>
        </div>
    </div>
</template>

<style scoped>
    .data-row {
        display: contents;
    }
    .data-row .container {
        grid-column: 1 / -1; 
        border-bottom: 1px solid var(--secondary-color);
    }
</style>