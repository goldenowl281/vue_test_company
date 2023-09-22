<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <header>
    <div class="wrapper">
      <nav class="navbar navbar-expand-lg bg-dark navbar-dark">
        <div class="container">
          <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav ml-auto">
              <li class="nav-item mx-3">
                <router-link class="nav-link active" aria-current="page" to="#">
                  Company
                </router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/employee">
                  Employee
                </router-link>
              </li>
            </ul>
          </div>
        </div>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
.navbar-collapse {
  display: flex;
  justify-content: center;
}
</style>
