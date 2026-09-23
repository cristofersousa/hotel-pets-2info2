<script setup>
import { RouterView } from 'vue-router';
import AppNavbar from '@/components/layout/AppNavbar.vue';
import AppSidebar from '@/components/layout/AppSidebar.vue';
</script>

<template>
  <AppNavbar />

  <div class="container-fluid">
    <div class="row">
      <aside class="col-md-3 col-lg-2 bg-body-tertiary border-end p-3 sidebar">
        <AppSidebar />
      </aside>

      <main
        id="conteudo"
        class="col-md-9 col-lg-10 px-3 px-md-4 py-4"
      >
        <RouterView />
      </main>
    </div>
  </div>
</template>

<style scoped>
@media (min-width: 768px) {
  .sidebar {
    min-height: calc(100vh - 56px);
  }
}
</style>
