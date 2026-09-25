### Criar Navbar, Sidebar e Layout com Bootstrap

- **`AppNavbar.vue`:** nome PetSchool e navegação principal.
- **`AppSidebar.vue`:** links para **Pets** e **Novo pet**;
- **`DefaultLayout.vue`:** Navbar no topo; abaixo, `container-fluid` com `row`, Sidebar em `col-md-3 col-lg-2` e conteúdo em `col-md-9 col-lg-10`.
- Em tela pequena, o menu pode ficar acima do conteúdo.

---

1. Crie diretório em `components/layout`

2. Crie os Arquivos:

- AppNavbar.vue` -> Barra Superior (logo, menu do usuário)
- AppSidebar.vue` -> Barra Lateral para navegação entre os dominios;
- DefaultLayout.vue` -> Centralizar o Layout para ser acionado no App.vue.

3.  Conteudo do AppNavbar:

```html
<script setup>
  import { RouterLink } from 'vue-router';
</script>

<template>
  <nav
    class="navbar navbar-expand-md navbar-dark bg-success"
    aria-label="Navegação principal"
  >
    <div class="container-fluid">
      <RouterLink
        class="navbar-brand fw-semibold"
        to="/pets"
      >
        <i
          class="bi bi-heart-fill me-2"
          aria-hidden="true"
        ></i>
        PetSchool
      </RouterLink>

      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#petschool-navbar"
        aria-controls="petschool-navbar"
        aria-expanded="false"
        aria-label="Abrir menu principal"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div
        id="petschool-navbar"
        class="collapse navbar-collapse"
      >
        <div class="navbar-nav ms-auto">
          <RouterLink
            class="nav-link"
            to="/pets"
            >Pets</RouterLink
          >
          <RouterLink
            class="nav-link"
            to="/pets/novo"
            >Novo pet</RouterLink
          >
        </div>
      </div>
    </div>
  </nav>
</template>
```

4. Conteudo do AppSidebar:

```html
<script setup>
  import { RouterLink } from 'vue-router';
</script>

<template>
  <nav aria-label="Menu da PetSchool">
    <div class="nav flex-column nav-pills gap-1">
      <RouterLink
        class="nav-link"
        active-class="active"
        to="/pets"
      >
        <i
          class="bi bi-heart me-2"
          aria-hidden="true"
        ></i>
        Pets
      </RouterLink>

      <RouterLink
        class="nav-link"
        active-class="active"
        to="/pets/novo"
      >
        <i
          class="bi bi-plus-circle me-2"
          aria-hidden="true"
        ></i>
        Novo pet
      </RouterLink>

      <span
        class="nav-link text-body-secondary"
        aria-disabled="true"
      >
        <i
          class="bi bi-people me-2"
          aria-hidden="true"
        ></i>
        Tutores <small class="ms-1">(em breve)</small>
      </span>
    </div>
  </nav>
</template>
```

5. Conteudo do DefaultLayout:

```html
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
```

6. Vamos chamar nosso arquivo DefaultLayout.vue no `src/App.vue`

```html

<script setup>
import DefaultLayout from '@/components/layout/DefaultLayout.vue';
</script>

<template>
  <DefaultLayout />
</template>

```
