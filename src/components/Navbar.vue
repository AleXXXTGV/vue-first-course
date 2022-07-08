<template>
    <div class="navbar">
        <div @click="$router.push('/')">Vue 3</div>
        <div class="navbar__btns">
            <my-button @click="$router.push('/posts')">Посты</my-button>
            <my-button style="margin-left: 20px;" @click="$router.push('/about')">О сайте</my-button>
            <my-button style="margin-left: 20px;" @click="$router.push('/store')">store</my-button>
            <my-button style="margin-left: 20px;" @click="$router.push('/composition')">Composition</my-button>
        </div>
    </div>
</template>
<script>
import router from '@/router/router'
export default {
    
}
</script>
<style>
    .navbar {
        height: 50px;
        background-color: lightgray;
        box-shadow: 2px 2px 4px gray;
        display: flex;
        align-items: center;
        padding: 0 15px;
    }
    .navbar__btns {
        margin-left: auto;
    }
</style>