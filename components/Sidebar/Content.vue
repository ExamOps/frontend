<script setup lang="ts">
    import settings from '~/assets/images/navbar/settings.svg';
    import projects from '~/assets/images/navbar/projects.svg';
    import proxies from '~/assets/images/navbar/proxies.svg';
    import home from '~/assets/images/navbar/home.svg';
    import logs from '~/assets/images/navbar/logs.svg';
    import { ProxyHelper } from '~/utils';
    import { ProxyAPI } from '~/api';

    const showSettings = defineModel<boolean>('showSettings', {
        type: Boolean,
        required: true,
    });
    
    const content = [
        { rus: 'Главная', eng: '/', path: home },
        { rus: 'Чемпионаты', eng: '/cups', name: 'cups' },
        { rus: 'Модули', eng: '/modules', path: proxies },
        { rus: 'Логи', eng: '/logs', path: logs, add: false }
    ];
    
    const createItem = reactive({
        show: false,
        name: '',
        data: {},
    });
    const proxyHelper = new ProxyHelper();

    const logout = () => {
        console.log('logout');
    };
</script>

<template>
    <SidebarContainer>
        <template #items>
            <SidebarLink
                v-for="item in content"
                :key="item.rus"
                :item="item"
                :create-item="createItem"
            />
        </template>

        <template #footer>
            <SidebarLink
                @click="showSettings = true"
                :item="{ rus: 'Версии', eng: '/notes', path: logs }"
                :create-item="createItem"
            />

            <SidebarLink
                @click="logout"
                :item="{ rus: 'Выйти', path: false, add: false }"
                :create-item="createItem"
            >
                <v-icon>mdi-minus-circle-outline</v-icon>
            </SidebarLink>
        </template>
    </SidebarContainer>
</template>