<script setup>
import { onMounted, ref } from 'vue';

const API_URL = 'http://localhost:3000';

const pets = ref([]);
const tutores = ref([]);

async function carregarDados() {
  const respostaPets = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPets.json();

  const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();
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
  </div>

  <table>
    <thead>
      <th>ID</th>
      <th>Nome</th>
      <th>Espécie</th>
      <th>Tutor</th>
    </thead>
    <tbody>
      <tr
        v-for="pet in pets"
        :key="pet.id"
      >
        <td>{{ pet.id }}</td>
        <td>{{ pet.nome }}</td>
        <td>{{ pet.especie }}</td>
        <td>
          {{
            tutores.find((t) => t.id === pet.tutorId)?.nome ||
            'Não especificado'
          }}
        </td>
      </tr>
    </tbody>
  </table>

  <!--

   Camilly
   Kauan
    Victor
    Kariny
    Irene

   -->
</template>
