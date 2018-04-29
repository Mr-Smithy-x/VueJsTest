<template>
    <div id="app">
        <nav>
            <router-link to="/">Home</router-link>
            <router-link to="/about">About</router-link>
            <router-link to="/news">News</router-link>
        </nav>
        <!--<img src="./assets/logo.png">-->
        <router-view :key="$route.fullPath"/>
        <footer>

        </footer>
    </div>
</template>

<script>

    export default {
        name: 'app',
    }
</script>

<style src="./app.css">

</style>
