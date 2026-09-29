<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

const router = useRouter(); // chamando meu router
// chamando a minha API
const API_URL = 'http://localhost:3000';

// lista vazia dos meus tutores
const tutores = ref([]);

// criar objeto para salvar um novo pet
// propriedades: nome, especie e tutorId

const novoPet = ref({
  nome: '',
  especie: '',
  tutorId: '',
});

// buscar todos os tutores que estão salvo na aplicação
async function carregarTutores() {
  const resposta = await fetch(`${API_URL}/tutores`);

  // converter os dados da minha API que estão em JSON para JS
  tutores.value = await resposta.json();
  console.table(tutores.value);
}

// salvar o novo pet no sistema
async function salvarPet() {
  await fetch(`${API_URL}/pets`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoPet.value),
  });

  // redirecionar para a tela de listagem de Pets
  router.push('/pets');
}

onMounted(carregarTutores);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastrar Pets</h1>
      <p class="text-body-secondary mb-0">Cadastro de Pets no sistema.</p>
    </header>

    <RouterLink
      class="btn btn-outline-primary"
      :to="{ name: 'pets' }"
    >
      Lista Pet
    </RouterLink>

    <form
      class="row"
      @submit.prevent="salvarPet"
    >
      <!-- nome do pet -->
      <div class="col-md-6">
        <label
          for="nome"
          class="form-label"
        >
          Nome do Pet:
        </label>
        <input
          type="text"
          class="form-control"
          id="nome"
          v-model="novoPet.nome"
          required
        />
      </div>
      <!-- especie do pet -->
      <div class="col-md-6">
        <label
          class="form-label"
          for="especie"
          >Espécie</label
        >
        <select
          name="especie"
          id="especie"
          class="form-select"
          required
          v-model="novoPet.especie"
        >
          <option
            value=""
            disabled
          >
            Selecione a espécie:
          </option>
          <option value="Cachorro">Cachorro</option>
          <option value="Gato">Gato</option>
          <option value="Coelho">Coelho</option>
          <option value="Tartaruga">Tartaruga</option>
        </select>
      </div>
      <!-- nome do tutor responsável pelo pet -->
      <div class="col-md-6">
        <label
          for="tutor"
          class="form-label"
          >Tutor</label
        >
        <select
          name="tutor"
          id="tutor"
          class="form-select"
          v-model="novoPet.tutorId"
        >
          <option
            value=""
            disabled
          >
            Selecione o Tutor
          </option>
          <option
            v-for="tutor in tutores"
            :key="tutor.id"
          >
            {{ tutor.nome }}
          </option>
        </select>
      </div>

      <div class="col-12 d-flex gap-2">
        <button
          class="btn btn-success"
          type="submit"
        >
          Salvar Pet
        </button>
      </div>
    </form>
  </div>
</template>
