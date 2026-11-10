<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import AtualizarTutorModal from './AtualizarTutorModal.vue';

const route = useRoute();
const tutor = ref(null);
const novoPet = ref({
  nome: '',
  especie: '',
  tutorId: '',
});

const pets = ref([]);

async function carregarTutor() {
  const { id } = route.params;
  const respostaTutor = await fetch(`http://localhost:3000/tutores/${id}`);
  tutor.value = await respostaTutor.json();
  console.table(tutor.value);
}

async function salvarPet() {
  if (!novoPet.value.nome || !novoPet.value.especie) {
    window.alert('Por favor, preencha todos os campos do pet.');
    return;
  }

  novoPet.value.tutorId = tutor.value.id;

  const resposta = await fetch('http://localhost:3000/pets', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoPet.value),
  });

  if (!resposta.ok) {
    window.alert('Ops, Não foi possível salvar o pet.');
    return;
  }

  // Limpar o formulário após salvar
  novoPet.value.nome = '';
  novoPet.value.especie = '';

  // Recarregar os dados do tutor para atualizar a lista de pets
  await carregarPets();
}

async function carregarPets() {
  const resposta = await fetch('http://localhost:3000/pets');

  if (!resposta.ok) {
    window.alert('Não foi possível carregar os pets.');
    return;
  }

  const todosPets = await resposta.json();

  pets.value = todosPets.filter(
    (pet) => String(pet.tutorId) === String(tutor.value.id),
  );
}

const modalAberto = ref(false);

async function tutorAtualizado() {
  await carregarTutor();
  modalAberto.value = false;
}

onMounted(async () => {
  await carregarTutor();
  await carregarPets();
});
</script>

<template>
  <div>
    <div class="row">
      <div class="col-md-12">
        <nav aria-label="breadcrumb">
          <ol class="breadcrumb">
            <li class="breadcrumb-item">
              <RouterLink to="/">Home</RouterLink>
            </li>
            <li
              class="breadcrumb-item"
              aria-current="page"
            >
              <RouterLink to="/tutores">Tutores</RouterLink>
            </li>
            <!-- <li class="breadcrumb-item active" aria-current="page">Tutor - {{ tutor.id }}</li> -->
          </ol>
        </nav>
      </div>
    </div>

    <div v-if="tutor">
      <section class="card mb-4">
        <div class="card-body">
          <section class="card mb-3">
            <div class="card-header">
              <h3 class="h5 mb-0">Dados do Tutor</h3>
            </div>

            <div class="card-body">
              <div class="row">
                <div class="col-md-4">
                  <p><strong>ID:</strong> {{ tutor.id }}</p>
                </div>
                <div class="col-md-4">
                  <p><strong>Nome:</strong> {{ tutor.nome }}</p>
                </div>
                <div class="col-md-4">
                  <p>
                    <strong>CPF/CNPJ:</strong>
                    {{ tutor.cpf_cnpj || 'N/A' }}
                  </p>
                </div>
              </div>

              <div class="row">
                <div class="col-md-4">
                  <p><strong>E-mail:</strong> {{ tutor.email || 'N/A' }}</p>
                </div>
                <div class="col-md-4">
                  <p>
                    <strong>Telefone:</strong>
                    {{ tutor.telefone || 'N/A' }}
                  </p>
                </div>
                <div class="col-md-4">
                  <p>
                    <strong>Data de Nascimento:</strong>
                    {{ tutor.dataNascimento || 'N/A' }}
                  </p>
                </div>
              </div>
            </div>
          </section>

          <section class="card">
            <div class="card-header">
              <h3 class="h5 mb-0">Endereço</h3>
            </div>

            <div class="card-body">
              <div class="row">
                <div class="col-md-4">
                  <p>
                    <strong>Rua:</strong> {{ tutor.endereco?.rua || 'N/A' }}
                  </p>
                </div>
                <div class="col-md-4">
                  <p>
                    <strong>Número:</strong>
                    {{ tutor.endereco?.numero || 'N/A' }}
                  </p>
                </div>
                <div class="col-md-4">
                  <p>
                    <strong>Bairro:</strong>
                    {{ tutor.endereco?.bairro || 'N/A' }}
                  </p>
                </div>
              </div>

              <div class="row">
                <div class="col-md-4">
                  <p>
                    <strong>Cidade:</strong>
                    {{ tutor.endereco?.cidade || 'N/A' }}
                  </p>
                </div>
                <div class="col-md-4">
                  <p>
                    <strong>Estado:</strong>
                    {{ tutor.endereco?.estado || 'N/A' }}
                  </p>
                </div>
                <div class="col-md-4">
                  <p>
                    <strong>CEP:</strong> {{ tutor.endereco?.cep || 'N/A' }}
                  </p>
                </div>
              </div>
            </div>
          </section>
        </div>

        <div
          class="d-flex justify-content-between align-items-center px-3 py-3"
        >
          <button
            type="button"
            class="btn btn-outline-secondary"
          >
            Cancelar
          </button>

          <button
            type="button"
            class="btn btn-primary"
            title="Atualizar Tutor"
            aria-label="Atualizar Tutor"
            @click="modalAberto = true"
          >
            <i
              class="bi bi-pencil-square"
              aria-hidden="true"
            ></i>
            Atualizar
          </button>
        </div>
      </section>

      <section class="card mb-4">
        <div class="card-body">
          <section class="card mb-3">
            <div class="card-header">
              <h3 class="h5 mb-0">Meus Pets</h3>
            </div>

            <div class="card-body">
              <div class="row">
                <div class="col-md-8">
                  <table class="table table-striped table-hover">
                    <thead>
                      <tr>
                        <th>ID</th>
                        <th>Nome</th>
                        <th>Espécie</th>
                        <th class="text-center">Ações</th>
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
                        <td class="text-center text-nowrap">
                          <button
                            type="button"
                            class="btn btn-outline-primary btn-sm me-1"
                            title="Atualizar pet"
                            aria-label="Atualizar pet"
                          >
                            <i
                              class="bi bi-pencil-square"
                              aria-hidden="true"
                            ></i>
                          </button>

                          <button
                            type="button"
                            class="btn btn-outline-danger btn-sm"
                            title="Remover pet"
                            aria-label="Remover pet"
                          >
                            <i
                              class="bi bi-trash-fill"
                              aria-hidden="true"
                            ></i>
                          </button>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>

                <div class="col-md-4">
                  <form
                    class="row"
                    @submit.prevent="salvarPet"
                  >
                    <!-- nome do pet -->
                    <div class="col-md-12">
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
                    <div class="col-md-12">
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
                    <div class="col-12 d-flex gap-2">
                      <button
                        class="btn btn-success mt-4"
                        type="submit"
                      >
                        Salvar Pet
                      </button>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </section>
        </div>
      </section>
    </div>
    <div v-else>
      <p>Carregando detalhes do tutor...</p>
    </div>
    <AtualizarTutorModal
      v-if="modalAberto && tutor"
      :tutor="tutor"
      @fechar="modalAberto = false"
      @salvo="tutorAtualizado"
    />
  </div>
</template>
