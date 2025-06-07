<script lang="ts" setup>
    import word from '~/assets/images/logos/word.png';
    // import excel from '~/assets/images/logos/excel.png';
    type Props = {
        title: string;
        type: string;
        weight: string;
        isLast: boolean;
        isDelete?: boolean;
    }
    const props = defineProps<Props>();

    const icons = computed(() => ({
        'Docx': word,
        'Doc': word,
    }) as any);
</script>

<template>
    <div class="file-container" :style="{ borderBottom: isLast ? '0px' : '1px solid var(--secondary-color)' }">
        <div class="left">
            <div class="icon-container">
                <img :src="icons[type]" alt="">
            </div>

            <div class="detail-container">
                <span class="title">{{ title }}</span>

                <span class="detail">{{ type }}  •  {{ weight }}</span>
            </div>
        </div>

        <div class="right">
            <button>
                <BaseIcon name="download"/>
            </button>

            <button v-if="isDelete">
                <BaseIcon name="delete"/>
            </button>
        </div>
    </div>
</template>

<style scoped>
    .file-container {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        padding: 10px 0;
    }
    .file-container .left {
        display: flex;
        flex-direction: row;
        gap: 10px;
    }
    .file-container .left .icon-container {
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .file-container .left .icon-container img {
        width: 30px;
        height: 30px;
    }
    .file-container .detail-container {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
    }
    .file-container .detail-container .title {
        font-size: 12px;
        font-weight: 700
    }
    .file-container .detail-container .detail {
        opacity: .7;
        font-size: 12px;
    }
    .file-container .right {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 5px;
    }
</style>