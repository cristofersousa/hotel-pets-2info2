<script setup>
import { computed, onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';

const tutores = ref([]);
const pets = ref([]);
const API_URL = 'http://localhost:3000';

async function carregarPets() {
  const resposta = await fetch(`${API_URL}/pets`);
  pets.value = await resposta.json();
}

//chamar a API para exibir todos os TUTORES
async function carregarTutores() {
  const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();
  console.table('TUTORES VALUE', tutores.value);
}

async function excluirTutor(id) {
  const confirmaExclusao = window.confirm(
    'Deseja realmente excluir esse Tutor?',
  );

  if (confirmaExclusao) {
    await fetch(`${API_URL}/tutores/${id}`, {
      method: 'DELETE',
    });
    await carregarTutores();
  }
}

const tutoresComPets = computed(() => {
  return tutores.value.map((tutor) => {
    const petsDoTutor = pets.value.filter(
      (pet) => String(pet.tutorId) === String(tutor.id),
    );

    return {
      ...tutor,
      quantidadePets: petsDoTutor.length,
      nomesPets: petsDoTutor.map((pet) => pet.nome).join(', ') || 'N/A',
    };
  });
});

onMounted(async () => {
  await carregarTutores();
  await carregarPets();
});
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Tutores</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Tutores cadastrados no sistema.
      </p>
    </header>

    <RouterLink
      class="btn btn-outline-primary"
      to="/tutores/novo"
    >
      Novo Tutor
    </RouterLink>

    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>#</th>
          <th>Nome</th>
          <th>CPF/CNPJ</th>
          <th>Telefone</th>
          <th>E-mail</th>
          <th class="text-center">Pets Quantidade</th>
          <th class="text-center">Pets Nome</th>
          <th class="text-center">Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="tutor in tutoresComPets"
          :key="tutor.id"
        >
          <td>{{ tutor.id }}</td>
          <td>{{ tutor.nome }}</td>
          <td>{{ tutor.cpf_cnpj || 'N/A' }}</td>
          <td>{{ tutor.telefone || 'N/A' }}</td>
          <td>{{ tutor.email || 'N/A' }}</td>
          <td>{{ tutor.quantidadePets || 'N/A' }}</td>
          <td>{{ tutor.nomesPets || 'N/A' }}</td>
          <td class="text-center text-nowrap">
            <RouterLink
              class="btn btn-outline-success btn-sm me-2"
              :to="`/tutores/${tutor.id}`"
            >
              <i
                class="bi bi-eye me-1"
                aria-hidden="true"
              ></i>
              Visualizar
            </RouterLink>

            <button
              class="btn btn-outline-danger btn-sm"
              type="button"
              @click="excluirTutor(tutor.id)"
            >
              <i
                class="bi bi-trash-fill me-1"
                aria-hidden="true"
              ></i>
              Deletar
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
