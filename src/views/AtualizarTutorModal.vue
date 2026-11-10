<script setup>
import { ref } from 'vue';

const props = defineProps({
  tutor: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(['fechar', 'salvo']);
// Copia os dados, incluindo o endereço.
// Cancelar não altera os dados exibidos pelo pai.

const formulario = ref({
  ...props.tutor,
  endereco: {
    cep: '',
    rua: '',
    numero: '',
    bairro: '',
    cidade: '',
    estado: '',
    ...props.tutor.endereco,
  },
});
const salvando = ref(false);
const erro = ref('');

async function buscaCep(cep) {
  const numeros = String(cep ?? '').replace(/\D/g, '');

  if (numeros.length !== 8) {
    erro.value = 'Oopss! Informe um CEP com 8 números.';
    return;
  }

  erro.value = '';
  try {
    const resposta = await fetch(
      `https://viacep.com.br/ws/${numeros}/json/`,
    );

    if (!resposta.ok) {
      throw new Error();
    }

    const dados = await resposta.json();

    if (dados.erro) {
      erro.value = 'CEP não encontrado.';
      return;
    }

    formulario.value.endereco.rua = dados.logradouro;
    formulario.value.endereco.bairro = dados.bairro;
    formulario.value.endereco.cidade = dados.localidade;
    formulario.value.endereco.estado = dados.uf;
  } catch {
    erro.value = 'Não foi possível consultar o CEP.';
  }
}

async function salvarTutor() {
  salvando.value = true;
  erro.value = '';

  const API_URL = import.meta.env.DEV
  ? 'http://localhost:3000'
  : 'https://hotel-pets-2info2.onrender.com';

  try {
    const resposta = await fetch(
      `${API_URL}/tutores/${props.tutor.id}`,
      {
        method: 'PATCH',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(formulario.value),
      },
    );

    if (!resposta.ok) {
      throw new Error('Não foi possível atualizar o tutor.');
    }

    emit('salvo');
  } catch {
    erro.value = 'Não foi possível atualizar o tutor. Tente novamente.';
  } finally {
    salvando.value = false;
  }
}
</script>

<template>
  <div>
    <div class="modal-backdrop show"></div>
    <div
      class="modal d-block"
      tabindex="-1"
      role="dialog"
      aria-modal="true"
      aria-labelledby="tituloAtualizarTutor"
    >
      <div class="modal-dialog modal-dialog-centered modal-lg">
        <form
          class="modal-content"
          @submit.prevent="salvarTutor"
        >
          <div class="modal-header">
            <h2
              id="tituloAtualizarTutor"
              class="modal-title fs-5"
            >
              Atualizar dados do tutor
            </h2>

            <button
              type="button"
              class="btn-close"
              aria-label="Fechar"
              :disabled="salvando"
              @click="emit('fechar')"
            ></button>
          </div>

          <div class="modal-body">
            <div
              v-if="erro"
              class="alert alert-danger"
              role="alert"
            >
              {{ erro }}
            </div>

            <div class="row g-3">
              <div class="col-md-4">
                <label
                  for="editarNomeTutor"
                  class="form-label"
                >
                  Nome
                </label>
                <input
                  id="editarNomeTutor"
                  v-model="formulario.nome"
                  class="form-control"
                  required
                />
              </div>

              <div class="col-md-4">
                <label
                  for="editarDocumentoTutor"
                  class="form-label"
                >
                  CPF/CNPJ
                </label>
                <input
                  id="editarDocumentoTutor"
                  v-model="formulario.cpf_cnpj"
                  class="form-control"
                />
              </div>

              <div class="col-md-4">
                <label
                  for="editarTelefoneTutor"
                  class="form-label"
                >
                  Telefone
                </label>
                <input
                  id="editarTelefoneTutor"
                  v-model="formulario.telefone"
                  type="tel"
                  class="form-control"
                  required
                />
              </div>

              <div class="col-md-4">
                <label
                  for="editarEmailTutor"
                  class="form-label"
                >
                  E-mail
                </label>
                <input
                  id="editarEmailTutor"
                  v-model="formulario.email"
                  type="email"
                  class="form-control"
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
                  v-model="formulario.endereco.cep"
                  required
                  @blur="buscaCep(formulario.endereco.cep)"
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
                  v-model="formulario.endereco.rua"
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
                  v-model="formulario.endereco.numero"
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
                  v-model="formulario.endereco.bairro"
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
                  v-model="formulario.endereco.cidade"
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
                  v-model="formulario.endereco.estado"
                  required
                />
              </div>
            </div>
          </div>

          <div class="modal-footer justify-content-between">
            <button
              type="button"
              class="btn btn-outline-secondary"
              :disabled="salvando"
              @click="emit('fechar')"
            >
              Cancelar
            </button>

            <button
              type="submit"
              class="btn btn-primary"
              :disabled="salvando"
            >
              {{ salvando ? 'Salvando...' : 'Salvar alterações' }}
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
