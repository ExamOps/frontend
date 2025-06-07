<script lang="ts" setup>
    type Props = {
        user: any;
    }
    const props = defineProps<Props>();
    const showDropdown = shallowRef(false);

    const actionControl = (key: string) => {
        if (showAction.value === key) {
            showAction.value = '';
            return;
        }

        showAction.value = key;
    }

    const showAction = shallowRef('');

    const actions = computed(() => ({
        'Интернет': 'checkbox',
        'Изменить': 'modal',
        'Удалить': 'confirm',
    }));

    const text = (action: string) => {
        if (actions.value[action as keyof typeof actions.value] === 'confirm' && showAction.value === action) 
            return 'Вы уверены?';
        return action;
    };
</script>

<template>
    <div class="user-container">
        <div class="entry first">
            <span>{{ user.first_name }} {{ user.last_name }} {{ user.patronymic }}</span>
        </div>

        <div class="entry">
            <span
                :class="{ 'active': user.ethernet }" 
                class="field"
            >Интернет</span>
        </div>
        
        <div class="entry">
            <span>Вход: <span class="mono">{{ user.login }}:{{ user.password }}</span></span>
        </div>
        
        <div class="entry">
            <span class="link">{{ user.server }}</span>
        </div>

        <div class="entry more">
            <div class="dropdown-wrapper">
                <button 
                    class="wrapper-button"
                    :class="{ 'active': showDropdown }"
                    @click="showDropdown = !showDropdown"
                >
                    <BaseIcon
                        :styles="{
                            'fontSize': '18px'
                        }" 
                        name="more"
                    />
                </button>

                <div v-if="showDropdown" class="dropdown-container">
                    <div
                        v-for="action in Object.keys(actions)"
                        :key="action"
                        class="action-container"
                    >
                        <button class="action-button" @click="actionControl(action)">
                            <BaseIcon 
                                :styles="{
                                    'fontSize': '18px'
                                }" 
                                :name="action"
                            />
                            <span>{{ text(action) }}</span>
                        </button>
                        
                        <div 
                            v-if="showAction === action" 
                            class="action"
                        >
                            <div v-if="actions[action as keyof typeof actions] === 'checkbox'" class="action-content">
                                <span>Сеть</span>
                                
                                <ModalsControlManyCheckbox 
                                    :name="action" 
                                    :scale=".8"
                                    v-model="user.ethernet"
                                />              
                            </div>
                            <div v-else-if="actions[action as keyof typeof actions] === 'confirm'" class="action-content">
                                <button 
                                    class="confirm cancel"
                                    @click="showAction = ''"
                                >
                                    Отмена
                                </button>
                                <button class="confirm accept">
                                    Удалить
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .user-container {
        background: var(--secondary-color);
        padding: 10px 20px;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        border-bottom: 1px dashed var(--secondary-color);
    }
    .user-container .entry {
        flex: 1;
        display: flex;
        justify-content: center;
    }
    .user-container .entry .mono {
        font-family: monospace;
    }
    .user-container .entry .link {
        text-decoration: underline;
        color: var(--active-color);
    }
    .user-container .entry.first {
        justify-content: flex-start;
    }
    .user-container .entry.more {
        display: flex;
        justify-content: flex-end;
    }
    .user-container .entry.more .dropdown-wrapper {
        position: relative;
        display: flex;
    }
    .user-container .entry.more .dropdown-wrapper .wrapper-button {
        background-color: var(--secondary-color);
        border-radius: 5px;
        padding: 2px;
        transition: all .3s linear;
        box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
    }
    .user-container .entry.more .dropdown-wrapper .wrapper-button.active {
        background-color: var(--secondary-sub-color);
    }
    .user-container .entry.more .dropdown-container {
        position: absolute;
        display: flex;
        flex-direction: column;
        background-color: var(--inversion-color);
        top: 25px;
        z-index: 4;
        right: 0;
        border: 1px solid var(--secondary-color);
        box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
        border-radius: 20px;
        padding: 4px;
        transition: all .3s linear;
    }
    .user-container .entry.more .dropdown-container .action-container {
        display: flex;
        flex-direction: column;
    }
    .user-container .entry.more .dropdown-container .action-button {
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
        align-items: center;
        padding: 6px 10px;
        gap: 5px;
    }
    .user-container .entry.more .dropdown-container .action-container:hover {
        transition: all .3s linear;
        background-color: var(--secondary-color);
    }
    .user-container .entry.more .dropdown-container .action-container:first-child  {
        border-radius: 15px 15px 0 0;
    }
    .user-container .entry.more .dropdown-container .action-container:last-child  {
        border-radius: 0 0 15px 15px;
    }
    .user-container .entry.more .dropdown-container .action-button span {
        font-weight: 700;
        font-size: 12px;
        white-space: nowrap;
    }
    .user-container .entry.more .dropdown-container .action {
        display: flex;
        flex-direction: row;
    }
    .user-container .entry.more .dropdown-container .action .action-content {
        display: flex;
        flex-direction: row;
        flex: 1;
        padding: 4px 10px;
        justify-content: space-between;
        gap: 5px;
    }
    .user-container .entry.more .dropdown-container .action .action-content .confirm {
        font-size: 12px;
        padding: 4px 10px;
        border-radius: 10px;
    }
    .user-container .entry.more .dropdown-container .action .action-content .confirm.cancel {
        border: 1px solid var(--secondary-color);
    }
    .user-container .entry.more .dropdown-container .action .action-content .confirm.accept {
        background: var(--delete-color);
        color: var(--inversion-color);
    }
    .user-container .entry.more .dropdown-container .action span {
        font-size: 12px;
    }
    .user-container .field.active {
        background: rgba(var(--accept-color-rgb), 0.15);
        color: var(--accept-color-text);
        border-color: var(--accept-color);
    }
    .user-container .field {
        font-size: 12px;
        background: rgba(var(--delete-color-rgb), 0.15);
        border: 1px solid var(--delete-color);
        color: var(--delete-color);
        border-radius: 20px;
        padding: 1px 10px;
    }
</style>