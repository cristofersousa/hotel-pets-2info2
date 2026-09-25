# Listar pets — `PetsView.vue`

Nesta etapa, vamos buscar os pets e os tutores cadastrados no JSON Server. A tela apresentará o nome, a espécie e o tutor de cada pet.

Esta é uma **primeira versão simplificada**. O objetivo é compreender o caminho percorrido pelos dados antes de adicionar tratamento de erros, mensagens de carregamento e outras melhorias.

## Antes de começar

A aplicação Vue e o JSON Server precisam estar em execução. O arquivo `db.json` deve conter as coleções `pets` e `tutores`.

Em um terminal, execute a aplicação Vue:

```bash
npm run dev
```

Em outro terminal, execute a API:

```bash
npm run api
```

Confira se estes endereços apresentam os dados:

- `http://localhost:3000/pets`
- `http://localhost:3000/tutores`

## Código da view

Crie o arquivo `src/views/PetsView.vue`:

```vue
<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';

const API_URL = 'http://localhost:3000';

// Listas que receberão os dados do JSON Server.
const pets = ref([]);
const tutores = ref([]);

// Busca primeiro os pets e depois os tutores.
async function carregarDados() {
  const respostaPets = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPets.json();

  const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();
}

// Recebe o tutorId do pet e procura o tutor correspondente.
function nomeDoTutor(tutorId) {
  for (const tutor of tutores.value) {
    if (tutor.id === tutorId) {
      return tutor.nome;
    }
  }

  return 'Tutor não encontrado';
}

// Executa carregarDados quando a view é aberta.
onMounted(carregarDados);
</script>

<template>
  <header
    class="d-flex flex-wrap align-items-center justify-content-between gap-3 mb-4"
  >
    <div>
      <h1 class="h2 mb-1">Pets cadastrados</h1>
      <p class="text-body-secondary mb-0">
        Conheça os pets e seus tutores.
      </p>
    </div>

    <RouterLink class="btn btn-success" to="/pets/novo">
      <i class="bi bi-plus-circle me-2" aria-hidden="true"></i>
      Novo pet
    </RouterLink>
  </header>

  <div class="table-responsive">
    <table class="table table-striped table-hover align-middle">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Nome</th>
          <th scope="col">Espécie</th>
          <th scope="col">Tutor</th>
        </tr>
      </thead>

      <tbody>
        <!-- v-for cria uma linha para cada pet da lista. -->
        <tr v-for="pet in pets" :key="pet.id">
          <th scope="row">{{ pet.id }}</th>
          <td>{{ pet.nome }}</td>
          <td>{{ pet.especie }}</td>

          <!-- Envia o tutorId para a função encontrar o nome do tutor. -->
          <td>{{ nomeDoTutor(pet.tutorId) }}</td>

        </tr>
      </tbody>
    </table>
  </div>
</template>
```

## Entendendo o código por partes

### 1. Criando as listas reativas

```js
const pets = ref([]);
const tutores = ref([]);
```

As duas variáveis começam como listas vazias. Quando os dados chegarem da API, seus valores serão atualizados.

Como foram criadas com `ref()`, utilizamos `.value` dentro do JavaScript:

```js
pets.value = await respostaPets.json();
```

No `<template>`, não precisamos escrever `.value`.

### 2. Buscando os pets

```js
const respostaPets = await fetch(`${API_URL}/pets`);
pets.value = await respostaPets.json();
```

O primeiro comando solicita os dados da rota `/pets`. O segundo transforma a resposta JSON em dados JavaScript e guarda a lista em `pets`.

### 3. Buscando os tutores

```js
const respostaTutores = await fetch(`${API_URL}/tutores`);
tutores.value = await respostaTutores.json();
```

O mesmo processo é realizado para obter a lista de tutores.

Nesta primeira versão, as requisições acontecem em sequência: primeiro pets e depois tutores.

### 4. Encontrando o nome do tutor

Cada pet possui um `tutorId`. A função percorre a lista de tutores até encontrar um tutor com o mesmo ID:

```js
function nomeDoTutor(tutorId) {
  for (const tutor of tutores.value) {
    if (tutor.id === tutorId) {
      return tutor.nome;
    }
  }

  return 'Tutor não encontrado';
}
```

O `for...of` observa um tutor de cada vez. Quando `tutor.id` for igual ao `tutorId` recebido, a função devolve o nome.

### 5. Executando a busca ao abrir a view

```js
onMounted(carregarDados);
```

`onMounted()` executa a função `carregarDados` quando a view é montada na tela.

### 6. Criando as linhas da tabela

```html
<tr v-for="pet in pets" :key="pet.id">
```

O `v-for` repete a linha para cada pet existente na lista. O `:key` fornece um identificador único ao Vue.

Dentro da linha, apresentamos os dados:

```html
<td>{{ pet.id }}</td>
<td>{{ pet.nome }}</td>
<td>{{ pet.especie }}</td>
<td>{{ nomeDoTutor(pet.tutorId) }}</td>
```

### 7. Exibindo o ID

A primeira coluna apresenta `pet.id`, o identificador único do pet no sistema:

```html
<th scope="row">{{ pet.id }}</th>
```

Esse mesmo ID será usado para visualizar ou deletar o pet correspondente.

### 8. Criando as ações da tabela

A coluna **Ações** contém os comandos relacionados ao pet daquela linha.

O botão **Visualizar** monta a rota utilizando o ID do pet:

```vue
<RouterLink :to="`/pets/${pet.id}`">
  Visualizar
</RouterLink>
```

Se o pet possui o ID `3`, o endereço criado será `/pets/3`.

O botão **Deletar** envia o ID para a função `excluirPet()`:

```vue
<button type="button" @click="excluirPet(pet.id)">
  Deletar
</button>
```


## Fluxo da funcionalidade

```text
A view é aberta
      ↓
onMounted executa carregarDados
      ↓
fetch busca /pets
      ↓
pets.value recebe a lista
      ↓
fetch busca /tutores
      ↓
tutores.value recebe a lista
      ↓
v-for cria uma linha para cada pet
      ↓
nomeDoTutor procura o tutor pelo tutorId
```

## Como funciona a relação entre pet e tutor?

Considere estes registros no `db.json`:

```json
{
  "tutores": [
    {
      "id": "1",
      "nome": "Ana"
    }
  ],
  "pets": [
    {
      "id": "1",
      "nome": "Safira",
      "especie": "Cachorro",
      "tutorId": "1"
    }
  ]
}
```

Safira guarda `tutorId: "1"`. A função recebe esse valor e percorre os tutores. Quando encontra Ana com `id: "1"`, devolve seu nome para a tabela.

Os dois valores são strings. Para que a comparação com `===` funcione, `id` e `tutorId` devem possuir o mesmo tipo.

## Resumo dos principais comandos

| Comando | O que faz |
| --- | --- |
| `ref([])` | Cria uma lista reativa. |
| `fetch('/pets')` | Solicita os pets para a API. |
| `resposta.json()` | Transforma a resposta JSON em dados JavaScript. |
| `for...of` | Percorre os tutores, um de cada vez. |
| `onMounted(carregarDados)` | Inicia a busca quando a view é aberta. |
| `v-for="pet in pets"` | Cria uma linha para cada pet. |
| `nomeDoTutor(pet.tutorId)` | Encontra o nome do tutor relacionado. |

## Para experimentar

1. Abra `/pets` e observe os pets apresentados na tabela.
2. Abra a aba **Rede/Network** do navegador e encontre as requisições `/pets` e `/tutores`.
3. Confira se a coluna **ID** apresenta o identificador de cada pet.
4. Clique em **Visualizar** e observe o ID presente na URL.
5. Cadastre um pet de teste e utilize **Deletar** para removê-lo.
6. Troque o `tutorId` de um pet por um ID inexistente e observe a mensagem **Tutor não encontrado**.

> **Atenção:** o botão **Visualizar** depende da rota `/pets/:id` e da view `PetDetailsView.vue`. Para testar a exclusão, crie um pet específico para essa finalidade e evite remover os registros iniciais da atividade.

## Melhorias futuras

Depois que esta versão estiver compreendida e funcionando, poderemos adicionar:

- mensagem de carregamento;
- mensagem para lista vazia;
- tratamento de erros com `try`, `catch` e `finally`;
- verificação de `resposta.ok`;
- execução paralela das requisições com `Promise.all()`;
- botão para tentar carregar os dados novamente.

Esses recursos melhoram a experiência da aplicação, mas não são necessários para compreender o primeiro fluxo de consulta e listagem.
