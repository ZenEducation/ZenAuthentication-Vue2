<template>
    <div style="display: flex; justify-content: center; align-items: center; height: 100vh;">
        <div>
            <h1 style="text-align: center; font-size: 2.5rem;">Profile</h1>
            <p style="text-align: center; font-size: 1.5rem; padding: 10px;">Hi {{ name }}! Welcome to your profile</p>
            <div style="display: flex; justify-content: center; align-items: center;"></div>
            <router-link to="/">
                <button style="width: 360px;">Sign Out</button>
            </router-link>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const name = ref('');

onMounted(() => {
    name.value = localStorage.getItem('name');
});
</script>
