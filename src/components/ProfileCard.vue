<script setup lang="ts">
import { ref } from 'vue'

defineProps<{ msg: string }>()

const count = ref(0)
</script>

<template>
    <div class="box">
        <div class="layout">
            <div class="side">
                <img src="../assets/me.jpg" alt="Picture of page author Henri Väisänen"/>
                <h3>Henri Väisänen</h3>
            </div>
            <div class="main">
                <h2>Hello there!</h2>
                <p>
                    I'm Henri Väisänen, a software developer based in Helsinki.
                </p>
                <p>
                    You can reach me on
                    <a href="https://www.linkedin.com/in/henrivaisanen/" target="_blank"
                    >LinkedIn</a
                    >
                    and checkout my freetime / study projects on my
                    <a href="https://github.com/henettaja" target="_blank">GitHub</a>.
                </p>
            </div>
        </div>
    </div>
</template>

<style scoped>
  .box {
      display: flex;
      flex-direction: column;
      gap: 0.5rem;
      max-width: 30rem;
      
      border-radius: 1rem;

      padding: 1rem 2rem;
      margin: 1rem 2rem;

      .layout {
          display: flex;
          flex-direction: row;
          flex-wrap: wrap;
          gap: 1.5rem;
          justify-content: center;

          .side, .main {
              display: flex;
              flex-direction: column;
              gap: 0.5rem;
              align-items: center;
          }

          .side {
              flex-basis: 30%;

              > h3 {
                  text-align: center;
              }

              > img {
                  width: 100px;
                  height: auto;
                  border-radius: 50%;
              }

          }

          .main {
              flex-basis: 60%;
              flex-grow: 1;
              flex-shrink: 0.75;

              > h2 {
                  align-self: center;
              }
          }
      }
  }
</style>
