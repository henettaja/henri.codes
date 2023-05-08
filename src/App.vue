<script setup lang="ts">
import ProfileCard from './components/ProfileCard.vue'
import MarqueeHeading from "./components/MarqueeHeading.vue";
</script>

<template>
    <main>
        <MarqueeHeading/>
        <section>
            <ProfileCard/>
        </section>
    </main>
</template>

<style scoped>
    main {
     min-height: 100vh;

     display: flex;
     flex-direction: column;
     justify-content: space-evenly;
    }

    section {
         display: flex;
         flex-direction: column;
         align-items: center;
         gap: 1rem;
    }
</style>
