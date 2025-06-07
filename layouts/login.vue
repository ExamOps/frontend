<template>
	<div id="notification"></div>
    
	<div id="background" class="container">
        <div id="content" class="main">
            <slot>
            </slot>
        </div>
	</div>
</template>

<style lang="scss" scoped>
    #background {
        background: var(--background);
    }
    .container {
        width: 100%;
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        position: relative;
    }
    .main {
        width: 700px;
        height: 500px;
        display: flex;
        flex-direction: row;
        border-radius: 15px;
        background: rgba(255, 255, 255, .9);
        backdrop-filter: blur(35px);
        box-shadow: 0 0 80px rgba(0, 0, 0, 0.15);
    }
    @media (max-width: 640px) {
        #content {
            height: 100vh;
            background: rgba(255, 255, 255, 0.2);
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
            backdrop-filter: blur(5px);
            -webkit-backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
    }
</style>