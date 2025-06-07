<script lang="ts" setup>

</script>

<template>
    <div class="upload-container">
        <BaseIcon 
            :style="{
                'fontSize': '50px'
            }"
            name="upload"
        />

        <div class="details">
            <h4>Перетащите файл</h4>
            <span>Для того, чтобы загрузить файл, вам надо перенсти его в эту область</span>
        </div>
    </div>
</template>

<style scoped>
    .upload-container {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        aspect-ratio: 2.5;
        border-radius: 20px;
        gap: 10px;
        border: 2px dashed var(--text-secondary);
        background: rgba(201, 214, 255, .3);
        background: -webkit-linear-gradient(to right, rgba(226, 226, 226, .5), rgba(201, 214, 255, .3));  /* Chrome 10-25, Safari 5.1-6 */
        background: linear-gradient(to right, rgba(226, 226, 226, .5), rgba(201, 214, 255, .3));

    }
    .upload-container .details {
        display: flex;
        gap: 5px;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
    .upload-container h4 {
        font-weight: 700;
    }
    .upload-container span {
        font-size: 12px;
        max-width: 70%;
        text-align: center;
    }
</style>