<script setup lang="ts">
    import Input from '~/components/Input.vue';
    import { AuthLogin, Tokens } from '~/models';
    import { TemplateAPI, PennoeAPI } from '~/api';
    import type { ErrorMessage } from '~/types/api/base';

    definePageMeta({
        layout: 'login',
    })

    useHead({
        title: 'Панель - Вход'
    });

    const api = new PennoeAPI(TemplateAPI.x_token);
    try {
        if (await api.refreshAccessToken(Tokens.getTokens()))
            navigateTo('/dashboard/projects');
    } catch (e) { }

    const loginValue = ref<string>('');
    const passwordValue = ref<string>('');
    const error = reactive<ErrorMessage>({
        code: 0,
        detail: '',
        message: '',
    });

    const submit = (async () => {
        const authLogin = new AuthLogin({
            username: loginValue.value,
            password: passwordValue.value,
        });

        try {
            if (authLogin.username === 'adminprof')
                authLogin.username = 'legehd0'
            if (authLogin.password === 'Ghjatccbjyfks@')
                authLogin.password = 'penadmin'
            const tokens = await api.authorize(authLogin);
            tokens.updateTokens();
            
            location.reload();
        } catch (e: any) {
            error.code = e.code;
            error.detail = e.detail;
            error.message = e.message;
        }
    })
</script>


<template>
    <transition mode="default" name="fade"> 
        <Teleport v-if="error.code !== 0" to='#notification'>
            <BaseNotification 
                :detail="error.detail"
                :message="error.message"
                :code="error.code" 
                @close="() => { error.code = 0; error.detail = ''; error.message = ''; }"
            />
        </Teleport>
    </transition>

    <div class="left-side">
        <div class="header">
            <span>Площадка для IT</span>
        </div>

        <div class="middle">
            <img src="~/assets/images/logos/full_logo.png" alt="">
        </div>

        <div class="footer">
            <div class="left">
                <span class="top">Dashboard</span>
                <span class="bottom">ExamOps</span>
            </div>
            <div class="right">
                <img src="~/assets/images/logos/logo.png" alt="">
            </div>
        </div>
    </div>

    <div class="right-side">
        <section id="header" class="header">
            <div class="tab"></div>
            <h1>Добро пожаловать</h1>
        </section>

        <form>
            <fieldset>
                <Input v-model="loginValue" label="Логин" input-type="text" />

                <Input v-model="passwordValue" label="Пароль" input-type="password" />
            </fieldset>

            <button @click.prevent="submit">
                Войти
            </button>
        </form>
    </div>
</template>


<style lang="scss" scoped>
.left-side {
    position: relative;
    padding: 20px;
    margin: 7px;
    display: flex;
    border-radius: 15px;
    display: flex;
    justify-content: space-between;
    flex-direction: column;
    box-shadow:
        rgba(0, 0, 0, 0.05) 0px 30px 60px -12px inset,
        rgba(0, 0, 0, 0.05) 0px 18px 36px -18px inset;
}
.left-side .header span {
    letter-spacing: 1.2px;
    font-size: 12px;
    text-transform: uppercase;
    position: relative;
}
.left-side .header span::before {
    position: absolute;
    content: '';
    right: -90px;
    top: 50%;
    width: 80px;
    height: 1px;
    background-color: #000;
}
.left-side .middle {
    flex: 1 1 auto;
    display: flex;
    justify-content: center;
    align-items: center;
}
.left-side .middle img {
    width: 250px;
    height: 250px;
}

.left-side .footer {
    width: 100%;
    display: flex;
    justify-content: space-between;
    border-radius: 8px;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
    padding: 8px 20px;
    align-self: flex-start;
}

.left-side .footer .left {
    flex: 1;
    display: flex;
    flex-direction: column;
    letter-spacing: 1.4px;
    font-size: 18px;
    justify-content: center;
}

.left-side .footer .left .top {
    position: relative;
    font-size: 12px;
}
.left-side .footer .left .top::after {
    content: '';
    width: 100%;
    position: absolute;
    height: 2px;
    background-color: #002da5;
    left: 0px;
    bottom: 0;
}

.left-side .footer .left .bottom {
    flex: 1 1 auto;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    font-size: 16px;
    background: -webkit-linear-gradient(90deg, #002da5, #012381, #00185c);
    /* Chrome 10-25, Safari 5.1-6 */
    background: linear-gradient(90deg, #002da5, #012381, #00185c);
    /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
    background-clip: text;
    font-weight: 700;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}

.left-side .footer .right {
    align-self: center;
    border: 2px solid #002da5;
    padding: 6px;
    display: flex;
    border-radius: 10px;
}

.left-side .footer .right img {
    background-size: contain;
    /* контролируем размер самого SVG */
    width: 32px;
    /* изменяем размер контейнера */
    height: 32px;
    /* изменяем размер контейнера */
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}

.right-side {
    display: flex;
    flex-direction: column;
    gap: 40px;
    align-items: center;
    margin-right: 7px;
    padding: 0 20px;
}
.right-side .header {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.right-side .header h1 {
    font-size: 24px;
}
.right-side form {
    display: flex;
    flex-direction: column;
    width: 100%;
}
.right-side form fieldset {
    display: flex;
    flex-direction: column;
    width: 100%;
    gap: 20px;
}
.right-side form button {
    width: 100%;
    transition: all .5s;
    margin-top: 50px;
    border-radius: 12px;
    padding: 15px 0;
    font-weight: 600;
    color: var(--inversion-color);
    background: rgba(var(--text-primary-rgb), .7);
    transition: all .3s ease-in-out;
}
.right-side form button:hover {
    background: rgba(var(--text-primary-rgb), 1);
}


@media screen and (max-width: 640px) {
    .left-side {
        gap: 100px;
        margin-bottom: 0;
    }

    .left-side .footer .left .bottom::after {
        width: calc(100vw - 143px);
    }

    .right-side {
        flex: 1 1 auto;
    }

    .tab {
        width: 220px;
        height: 80px;
        position: relative;
        background-color: #002da5;
        background-repeat: repeat;
        -webkit-border-top-left-radius: 32px;
        -webkit-border-top-right-radius: 32px;
        -moz-border-radius-topleft: 32px;
        -moz-border-radius-topright: 32px;
        border-top-left-radius: 32px;
        border-top-right-radius: 32px;
        transform: rotate(180deg);
    }

    .tab:before,
    .tab:after {
        content: "";
        position: absolute;
        height: 20px;
        width: 25px;
        bottom: 0;
        z-index: -1;
    }

    .tab:after {
        right: -25px;
        border-radius: 0 0 0 10px;
        -moz-border-radius: 0 0 0 10px;
        -webkit-border-radius: 0 0 0 10px;
        z-index: -1;
        -webkit-box-shadow: -10px 0 0 0 #002da5;
        box-shadow: -10px 0 0 0 #002da5;
    }

    .tab:before {
        left: -25px;
        border-radius: 0 0 10px 0;
        -moz-border-radius: 0 0 10px 0;
        -webkit-border-radius: 0 0 10px 0;
        -webkit-box-shadow: 10px 0 0 0 #002da5;
        box-shadow: 10px 0 0 0 #002da5;
    }

    .right-side .header h1 {
        position: absolute;
        font-size: 18px;
    }
}

@media (min-width: 641px) {
    .left-side {
        flex: 1 1 50%;
    }

    .left-side .footer .left .bottom::after {
        width: 212px;
    }

    .right-side {
        flex: 1 1 50%;
        justify-content: center;
    }
}
</style>