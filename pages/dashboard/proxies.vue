<script setup lang="ts">
    import { ProxyAPI, TemplateAPI } from '~/api';
    import { Tokens } from '~/models';

    const route = useRoute();
    const router = useRouter();

    const tokens = new Tokens(Tokens.getTokens());
    const proxiesAPI = new ProxyAPI(
        TemplateAPI.x_token,
        tokens.access_token, 
    );    
    const showModal = shallowRef<boolean>(false);
    const showNotification = shallowRef<boolean>(false);

    const dump = new Dump({
        page: route.query.page,
    }, ['page'], proxiesAPI, tokens);

    const dumpData = ref(await dump.dump());
    const totalPages = computed(() => dumpData.value.maxPages);
    const count = computed(() => dumpData.value.count);

    watch(() => route.fullPath, async () => {
        dump.refreshRequestParams(route.query);
        dumpData.value = await dump.dump();
    });

    const proxyHelper = new ProxyHelper();

    const actions = computed(() => ({
        'Добавить несколько': {
            request: new ProxyAPI('').create,
            params: proxyHelper.proxyParams
        }
    }));
</script>

<template>
    <Head>
        <title>Прокси</title>
    </Head>

    <DashboardHeader 
        :title="'Прокси ' + (count ? ` (${count})` : '')"
        :paths="['Панель', 'Прокси']"
    />

    <DashboardCategory title="Таблица">        
        <ActionsContent :border-bottom="true">
            <ActionsAction
                v-for="action in Object.keys(actions)"
                :key="action"
                :title="action"
                @click="showModal = true"
            >
                <Teleport 
                    to="#modal" 
                    v-if="showModal"
                >   
                    <ModalsControlModal 
                        @close="showModal = false"
                        title="Добавление нескольких прокси"
                        :outline="false"
                    >
                        <template #content>
                            <ModalsControlManyForm
                                name="Создание нескольких прокси"
                                model="Proxy"
                                @close="showModal = false"
                                :api="new ProxyAPI('')"
                                :call="actions[action as keyof typeof actions].request"
                                :params="actions[action as keyof typeof actions].params"
                                :get-params="proxyHelper.getUpdateParams"
                                :helper="proxyHelper"
                                :styles="{
                                    'padding': '0',
                                    'padding-top': '15px',
                                    'max-height': '580px',
                                    'overflow-y': 'auto',
                                }"
                            />
                        </template>
                    </ModalsControlModal>
                </Teleport>
            </ActionsAction>
        </ActionsContent>

        <main>
            <ProxiesCard 
                v-for="proxy in dumpData.data" 
                :key="proxy.id" 
                :proxy="proxy"
                @data-changed="showNotification = true"
            />
        </main>
    </DashboardCategory>
    

    <transition mode="default" name="fade"> 
        <Teleport 
            v-if="showNotification" 
            to='#notification'
    >
            <BaseNotification 
                detail="Обновление данных"
                message="На данной странице данные не обновляются. Чтобы посмотреть изменения - обновите страницу."
                :code="101" 
                @close="showNotification = false"
            />
        </Teleport>
    </transition>

    <BasePagination
        v-if="totalPages > 1"
        :total-pages="totalPages"
    />
</template>

<style scoped>
    main {
        margin-top: 20px;
        padding: 20px;
        padding-top: 0;
        display: flex;
        flex-direction: row;
        gap: 15px;
        flex-wrap: wrap;
    }
    .fade-enter-active,
    .fade-leave-active {
        transition: opacity 0.5s ease;
    }
    .fade-enter-from,
    .fade-leave-to {
        opacity: 0;
    }
</style>