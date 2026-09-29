<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';

const pets = ref([]); // lista de pets vazia
const tutores = ref([]); // lista de tutores vazia;

// chamando a minha API geral:
const API_URL = 'http://localhost:3000';

// chamar a minha API para listar todos os pets e tutores;
async function carregarDados() {
  const respostaPets = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPets.json();

  const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();

  console.table(pets.value);
  console.table(tutores.value);
}

// exibir o nome do tutor
function nomeDoTutor(tutorId) {
  for (const tutor of tutores.value) {
    if (tutor.id === tutorId) {
      return tutor.nome;
    }
  }
  return 'Oopps, Tutor não encontrado!';
}

onMounted(carregarDados);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Pets cadastrados no sistema.
      </p>
    </header>

    <RouterLink
      class="btn btn-primary btn-outline"
      to="/pets/novo"
    >
      Novo pet
    </RouterLink>

    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>id</th>
          <th>Nome</th>
          <th>Espécie</th>
          <th>Tutor</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="pet in pets"
          :key="pet.id"
        >
          <td>{{ pet.id }}</td>
          <td>{{ pet.nome }}</td>
          <td>{{ pet.especie }}</td>
          <td>{{ nomeDoTutor(pet.tutorId) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
