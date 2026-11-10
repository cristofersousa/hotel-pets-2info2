<script setup>
import { ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

const router = useRouter(); // chamando meu router
const API_URL = 'http://localhost:3000'; // chamando a minha API

const novoTutor = ref({
  nome: '',
  cpf_cnpj: '',
  telefone: '',
  email: '',
  dataNascimento: '',
  observacoes: '',
  endereco: {
    rua: '',
    numero: '',
    bairro: '',
    cidade: '',
    estado: '',
    cep: '',
  },
});

const cepApi = ref({});

async function buscaCep(cep) {
  const API_CEP = 'https://viacep.com.br/ws';
  const respostaCep = await fetch(`${API_CEP}/${cep}/json`);
  cepApi.value = await respostaCep.json();
  console.table(cepApi.value);
  novoTutor.value.endereco.rua = cepApi.value.logradouro;
  novoTutor.value.endereco.bairro = cepApi.value.bairro;
  novoTutor.value.endereco.cidade = cepApi.value.localidade;
  novoTutor.value.endereco.estado = cepApi.value.uf;
}

// salvar o novo Tutor no sistema
async function salvarTutor() {
  const resposta = await fetch(`${API_URL}/tutores`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoTutor.value),
  });

  if (!resposta.ok) {
    window.alert('Não foi possível salvar o tutor.');
    return;
  }
  // redirecionar para a tela de listagem de Tutores
  router.push('/tutores');
}
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastrar Tutores</h1>
      <p class="text-body-secondary mb-0">Cadastro de Tutores no sistema.</p>
    </header>

    <RouterLink
      class="btn btn-outline-primary mb-2"
      :to="{ name: 'tutores' }"
    >
      Lista Tutores
    </RouterLink>

    <form
      class="row"
      @submit.prevent="salvarTutor"
    >
      <div class="row">
        <div class="col-md-12">
          <h4 class="text-2xl font-bold">Dados Gerais</h4>
        </div>
      </div>
      <!-- nome do Tutor -->
      <div class="row">
        <div class="col-md-4">
          <label
            for="nome"
            class="form-label"
          >
            Nome Completo:
          </label>
          <input
            type="text"
            class="form-control"
            id="nome"
            v-model="novoTutor.nome"
            required
          />
        </div>

        <div class="col-12 col-md-4">
          <!-- CPF ou CNPJ -->
          <label
            for="cpf"
            class="form-label"
          >
            CPF /CNPJ:
          </label>
          <input
            type="text"
            class="form-control"
            id="cpf"
            v-model="novoTutor.cpf_cnpj"
            required
          />
        </div>
        <!-- Telefone do Tutor -->
        <div class="col-md-4">
          <label
            for="telefone"
            class="form-label"
          >
            Telefone / WhatsApp:
          </label>
          <input
            type="text"
            class="form-control"
            id="telefone"
            v-model="novoTutor.telefone"
            required
          />
        </div>
      </div>

      <div class="row">
        <!-- Data De Nascimento do Tutor -->
        <div class="col-md-4">
          <label
            for="nome"
            class="form-label"
          >
            Data de Nascimento
          </label>
          <input
            type="text"
            class="form-control"
            id="dataNascimento"
            v-model="novoTutor.dataNascimento"
            required
          />
        </div>

        <div class="col-12 col-md-8">
          <!-- Observações -->
          <label
            for="observacoes"
            class="form-label"
          >
            Observações:
          </label>
          <textarea
            class="form-control"
            id="observacoes"
            v-model="novoTutor.observacoes"
            required
          />
        </div>
      </div>

      <div class="row">
        <div class="col-md-12">
          <h4 class="text-2xl font-bold">Endereço</h4>
        </div>
      </div>

      <!-- CEP do Tutor -->
      <div class="row">
        <div class="col-md-4">
          <label
            for="cep"
            class="form-label"
          >
            CEP
          </label>
          <input
            type="text"
            class="form-control"
            id="cep"
            v-model="novoTutor.endereco.cep"
            required
            @blur="buscaCep(novoTutor.endereco.cep)"
          />
        </div>

        <div class="col-12 col-md-4">
          <!-- RUA -->
          <label
            for="rua"
            class="form-label"
          >
            Rua:
          </label>
          <input
            type="text"
            class="form-control"
            id="rua"
            v-model="novoTutor.endereco.rua"
            required
          />
        </div>
        <!-- Telefone do Tutor -->
        <div class="col-md-4">
          <label
            for="numero"
            class="form-label"
          >
            Número:
          </label>
          <input
            type="text"
            class="form-control"
            id="numero"
            v-model="novoTutor.endereco.numero"
            required
          />
        </div>
      </div>

      <div class="row">
        <div class="col-md-4">
          <label
            for="bairro"
            class="form-label"
          >
            Bairro
          </label>
          <input
            type="text"
            class="form-control"
            id="bairro"
            v-model="novoTutor.endereco.bairro"
            required
          />
        </div>

        <div class="col-12 col-md-4">
          <!-- Cidade -->
          <label
            for="cidade"
            class="form-label"
          >
            Cidade:
          </label>
          <input
            type="text"
            class="form-control"
            id="cidade"
            v-model="novoTutor.endereco.cidade"
            required
          />
        </div>
        <!-- Estado -->
        <div class="col-md-4">
          <label
            for="estado"
            class="form-label"
          >
            Estado:
          </label>
          <input
            type="text"
            class="form-control"
            id="estado"
            v-model="novoTutor.endereco.estado"
            required
          />
        </div>
      </div>

      <div class="col-12 d-flex gap-2">
        <button
          class="btn btn-success mt-2"
          type="submit"
        >
          Salvar Tutor
        </button>
      </div>
    </form>
  </div>
</template>
