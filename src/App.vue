<template>
    <div id="app" class="h-screen w-full bg-blue-200 flex items-center justify-center">
        <credit-card></credit-card>
    </div>
</template>

<script>

    import CreditCard from './components/CreditCard';

    export default {
        name: 'app',
        components: {
            CreditCard
        }
    }
</script>

<style>
    @import "assets/css/app.css";
</style>
