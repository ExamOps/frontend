<script lang="ts" setup>
    type Props = {
        showTitle: boolean;
    }
    defineProps<Props>();
</script>

<template>
    <section class="logo-container">
        <div class="img-container">
            <img src="~/assets/images/logos/logo.png" alt="Пена">
        </div>

        <h1 v-if="showTitle">ExamOps</h1>
    </section>
</template>

<style scoped>
    .logo-container {
        display: flex;
        flex-direction: row;
        gap: 10px;
        align-items: center;
        padding: 30px;
    }
    .logo-container .img-container img {
        max-width: none;
        width: 38px;
        height: 38px;
    }
    h1 {
        font-size: 18px;
        font-weight: 700;
        letter-spacing: .3px;
        background: -webkit-linear-gradient(90deg, #002da5, #012381, #00185c); /* Chrome 10-25, Safari 5.1-6 */
        background: linear-gradient(90deg, #002da5, #012381, #00185c); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
        background-clip: text;
        color: transparent;
    }
</style>