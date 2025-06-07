<script lang="ts" setup>
    import { PennoeAPI, TemplateAPI } from '~/api';
    import {
        ServiceItem, 
        Tokens,
        Filter,
        Statistic
    } from '~/models';
    import type { TableMode } from '~/types';
    import { initCupsData, initModulesData, initUsersData } from '~/utils';

    const route = useRoute();

    const serviceItem = {
        code: 'durak',
        name: 'Дурак Онлайн'
    }

    /** Registration of services */
    const serviceStrategy = new ServicesStrategy();	

    /** Strategy methods */
    const serviceData = computed(() => ({
        code: serviceItem.code,
        mode: activeMode.value
    }));

    /** Strategy methods */
    const api = computed(() => serviceStrategy.getAPI(serviceData.value));
    const call = (params: any) => serviceStrategy.getDumpRequest(serviceData.value, params);
    const updateCall = (params: any) => serviceStrategy.getUpdateRequest(serviceData.value, params);
    const updateParams = computed(() => serviceStrategy.getUpdateRequestParams(serviceData.value));
    const modes = computed(() => serviceStrategy.getModes(serviceData.value));
    const activeMode = shallowRef(route.query.mode as string || 'accounts');
    const displayKeys = computed(() => serviceStrategy.getDisplayKeys(serviceData.value));
    const isRelink = computed(() => serviceStrategy.getRelinkRequestParams(serviceData.value));
    const isRefresh = computed(() => serviceStrategy.getRefreshRequestParams(serviceData.value));
    
    /** Init service store */
    // const serviceStore = useServiceStore();
    // serviceStore.updateParams({
    //     tableMode: activeMode.value,
    //     serviceCode: serviceItem.code,
    //     isRefresh: isRefresh.value,
    //     isRelink: isRelink.value,
    //     requestParams: updateParams.value,
    // });

    /** Table dump methods */
    // const dumpStore = useDumpStore();
    // dumpStore.initializate({
    //     filter_by: route.query.filter_by,
    //     order: route.query.order,
    //     query: route.query.query,
    //     page: route.query.page, 
    // }, ['string', 'string', 'string', 'page']);
    
    const stats: Statistic = await serviceStrategy.getStats(serviceData.value) as Statistic;
    
    // const getDisplayData = () => {
    //     const keys = displayKeys.value;
    //     return table.value.map((item: any) => {
    //         return Object.entries(item).reduce((acc: any, [key, value]) => {
    //             if (keys.includes(key)) {
    //                 acc[key] = value;
    //                 if (key.split('_').length > 1  && key.split('_')[1] === 'at') {
                        
    //                     acc[key] = value !== 0 
    //                         ? new Date(value as number * 1_000).toLocaleString()
    //                         : "—";
    //                 }
    //             } 
    //             return acc;
    //         }, {});
    //     });
    // }    

    /** Dump methods */
    // const dump = async () => {
    //     const params = dumpStore.params;
    //     const dumpResult = await call(params);

    //     dumpStore.table = structuredClone(dumpResult.data);
    //     dumpStore.maxPages = Math.ceil(dumpResult.count / 25);
    //     dumpStore.count = dumpResult.count;
    // }
    // await dump();

    // const table = computed(() => dumpStore.table);
    // const totalPages = computed(() => dumpStore.maxPages);
    // const count = computed(() => dumpStore.count);
    // const params = computed(() => dumpStore.params);
    // const displayData = ref(getDisplayData());
    
    // const columns = computed(() => Object.keys(displayData.value[0]));
    // const modesIcons = computed(() => ({
    //     'accounts': {
    //         icon: 'fa-solid fa-users',
    //         iconType: 'fa',
    //     },
    //     'streaks': {
    //         icon: 'fa-solid fa-barcode',
    //         iconType: 'fa',
    //     },
    // }));

    // /** After route params change */
    // watch(() => route.fullPath, async() => {    
    //     dumpStore.refreshRequestParams(route.query);
    //     await dump();
    //     serviceStore.updateParams({
    //         tableMode: route.query.mode as TableMode || 'accounts',
    //         serviceCode: serviceItem.code,
    //         isRefresh: isRefresh.value,
    //         isRelink: isRelink.value,
    //         requestParams: updateParams.value,
    //     });
    // });

    // /** Display data methods */
    // watch(table, () => {
    //     displayData.value = getDisplayData();
    // });

    /** Actions */
    const actionConfirmation = shallowRef<boolean>(false);
    const acceptDelete = shallowRef<boolean | undefined>(undefined);
    const actions = computed(() => serviceStrategy.getActions(serviceData.value));
    const actionsShow = reactive(Object.entries(actions.value).reduce((acc: any, [key, value]) => {
        acc[key] = false;
        return acc;
    }, {}));

    watch(acceptDelete, (value) => {
        Object.keys(actionsShow).forEach((action: any) => {
            actionsShow[action] = false;
        });

        if (value) {

            /** Wipe Request */
            /** ... */
        }

        acceptDelete.value = undefined; 
    });

    if (!localStorage.getItem('cups')) {
        localStorage.setItem('cups', JSON.stringify(initCupsData));
    }

    if (!localStorage.getItem('modules')) {
        localStorage.setItem('modules', JSON.stringify(initModulesData));
    }

    if (!localStorage.getItem('users')) {
        localStorage.setItem('users', JSON.stringify(initUsersData));
    }

    const cupsLocalStorage = computed(() => JSON.parse(localStorage.getItem('cups') as string));
    const modulesLocalStorage = computed(() => JSON.parse(localStorage.getItem('modules') as string));
    const usersLocalStorage = computed(() => JSON.parse(localStorage.getItem('users') as string));

    const cupsData = ref(cupsLocalStorage.value);
    const modulesData = ref(modulesLocalStorage.value);
    const usersData = ref(usersLocalStorage.value);
    const activeCup = ref<string>(initCupsData[0].cup);
    const modulesActiveCup = computed(() => modulesData.value.filter((module: any) => module.cup == activeCup.value)[0].modules);
    const usersActiveCup = computed(() => usersData.value.filter((module: any) => module.cup == activeCup.value)[0].users);
    
    const className = (index: number, data: any[]) => {
        if (index === 0) return 'first';
        if (index === data.length - 1) return 'last';
        return '';
    }
    
    const borderBottom = (index: number, data: any[]) => {
        if (data.length < 25)
            return index === data.length - 1 ? 'none' : '1px solid var(--secondary-color)';
            
        return index === data.length - 1 ? 'none' : '1px solid var(--secondary-color)';
    };

    const files = ref([{
        type: 'Docx',
        title: 'Пояснительная записка',
        weight: '8.5MB'
    },{
        type: 'Docx',
        title: 'Пояснительная записка',
        weight: '8.5MB'
    },]);

    const cupModules = computed(() => cupsData.value.map((cupData: any) => {
        delete cupData.first_name;
        delete cupData.last_name;
        delete cupData.patronymic;
        delete cupData.login;
        delete cupData.password;
        cupData.indicators = indicators.value;
        return cupData;
    }));
    const indicators = computed(() => ([
        'MySQL',
        'SAMBA',
        'FTP',
        'SSH'
    ]));

    const params = computed(() => ({
        'last_name': {
            example: 'Иванов',
            type: 'string',
            required: true,
        },
        'first_name': {
            example: 'Иван',
            type: 'string',
            required: true,
        },
        'patronymic': {
            example: 'Иванович',
            type: 'string',
            required: true,
        },
        'login': {
            example: 'st1992',
            type: 'string',
            required: true,
        },
        'password': {
            example: 'pwd1992',
            type: 'string',
            required: true,
        },
        'cup': {
            example: 'Be the Third',
            type: 'string',
            required: true,
        },
        'count': {
            example: 2,
            type: 'string',
            required: true,
        },
    }));

    const commands = ref([
        'cd',
        'exit',
        'ls',
        'mysql',
        'echo',
        'git',
        'ssh',
        'pip',
        'nano',
        'cp',
        'touch',
        'node',
        'npm'
    ]);

    const add = (element: string) => {
        const exist = commands.value.indexOf(element);

        if (exist === -1)
            commands.value.push(element);
    } 
</script>

<template>
    <Head>
        <Title>Модули | ExapOps</Title>
    </Head>

    <DashboardHeader 
        title="Модули чемпионатов"
        :paths="['modules']"
    />

    <DashboardCategory 
        title="Выбранный чемионат"
        :contentStyles="{
            'flexDirection': 'column',
            'gap': '10px'
        }"
    >
        <CommandsContent 
            @click="(value: any) => commands.splice(commands.indexOf(value), 1)"
            :commands="commands"
        >
            <CommandsInput @add="(value: any) => add(value)"/>
        </CommandsContent>

        <div class="row">
            <TableContent 
                title="Модули"
                :data="modulesActiveCup" 
                :call="updateCall"
                :api="api"
                :styles="{ 'flex': '1 1 50%' }"
            >
                <TableDataRow 
                    v-for="(row, index) in modulesActiveCup" 
                    :key="row.id"
                    :item="row"
                    :call="call"
                    :api="api"
                >
                    <template #columns>
                        <TableDataColumn 
                            v-for="(column, columnIndex) in Object.keys(row)" 
                            :key="column" 
                            :class-name="className(columnIndex, Object.keys(row))"
                            :name="column"
                            :field="'Модуль ' + row[column as keyof typeof row]"
                            :is-last-row="index === modulesActiveCup.length - 1"
                            :style="{
                                borderBottom: borderBottom(index, modulesActiveCup),
                            }"
                        >   
                            <ModalsControlManyCheckbox 
                                v-if="column === 'status'"
                                :name="'status_' + index" 
                                :scale="1"
                                v-model="row[column]"
                            />
                        </TableDataColumn>
                    </template>
                </TableDataRow>
            </TableContent>
            
            <FilesContent
                :is-delete="true"
                :files="files"
                :styles="{
                    'minHeight': '250px'
                }"
            >
                <FilesUpload/>
            </FilesContent>
        </div>

        </DashboardCategory>

    <DashboardCategory 
        title="Таблица"
        :styles="{
            'gap': '10px',
        }"
    >
        <ActionsContent :borderBottom="true">
            <ActionsAction
                v-for="action in Object.keys(actions)"
                :key="action"
                :title="action"
                @click="actionsShow[action] = true"
        >
            <Teleport
                v-if="actionsShow[action]"
                to="#modal"
            >
                <ModalsControlModal 
                    :title="action + ' ' + (activeMode === 'accounts' ? 'аккаунт' : 'штрих')"
                    :height="'500px'"
                    @close="actionsShow[action] = false"
                >
                    <template #content>
                        <ModalsControlForm
                            :name="action + ' ' + (activeMode === 'accounts' ? 'аккаунт' : 'штрих')"
                            :model="activeMode === 'accounts' ? 'Account' : 'Streak'"
                            @close="actionsShow[action] = false"
                            :api="api"
                            :call="actions[action].request"
                            :params="params"
                            :styles="{
                                'padding': '0',
                                'padding-top': '15px',
                            }"
                            keyof="cups"
                        />
                    </template>
                    </ModalsControlModal>
                </Teleport>
            </ActionsAction>
        </ActionsContent>

        <main>
            <TableContent 
                title="Чемпионатные модули"
                :data="cupModules" 
                :call="updateCall"
                :api="api"
                :is-expanded="true"
                v-model="activeCup"
            >
                <template #expanded>
                    <TableDetailExpanded
                        v-for="user in usersActiveCup"
                        :key="user"
                        :user="user"
                    />
                </template>
            </TableContent>
        </main>
    </DashboardCategory>
</template>

<style scoped>
    main {
        flex: 1 1 auto;
        display: flex;
        flex-direction: column;
        overflow-x: auto;
        min-width: 700px;
    }
    .row {
        display: flex;
        flex-direction: row;
        gap: 10px;
    }
    .controlbar-content {
        padding: 20px;
        display: flex;
        flex-direction: column;
        gap: 20px;
        border-bottom: 2px solid var(--secondary-color);
    }
    @media screen and (max-width: 620px) {
        .searchBox:hover > .searchInput {
            width: 150px;
            padding: 0 6px;
        }
    }
    @media screen and (max-width: 1350px) {
        main { 
            margin: 0;
        }
    }
</style>