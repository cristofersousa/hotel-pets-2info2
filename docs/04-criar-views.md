1. Crie o arquivo `views/AddPetView`

```html
<template>
  <header class="mb-4">
    <h1 class="h2">Pets cadastrados</h1>
    <p class="text-body-secondary mb-0">
      Aqui ficará a lista de pets e seus respectivos tutores.
    </p>
  </header>

  <RouterLink
    class="btn btn-success"
    to="/pets/novo"
    >Cadastrar pet</RouterLink
  >
</template>

```


2. Crie o Arquivo PetsViews.vue

```html
<template>
  <header class="mb-4">
    <h1 class="h2">Pets cadastrados</h1>
    <p class="text-body-secondary mb-0">
      Aqui ficará a lista de pets e seus respectivos tutores.
    </p>
  </header>

  <RouterLink
    class="btn btn-success"
    to="/pets/novo"
    >Cadastrar pet</RouterLink
  >
</template>

```

3. Configure o arquivo de rotas para receber os novos templates criados, acesse `router/index.js`


```js
import { createRouter, createWebHistory } from 'vue-router';

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      redirect: '/pets', // listar todos os pets cadastrados
    },
    {
      path: '/pets',
      name: 'pets',
      component: () => import('../views/PetsView.vue'),
    },
    {
      path: '/pets/novo', // exibir a tela para cadastrar um novo pet
      name: 'novo-pet', 
      component: () => import('../views/AddPetView.vue'),
    },
  ],
});

export default router;
```
