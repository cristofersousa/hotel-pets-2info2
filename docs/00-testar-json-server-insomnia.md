# Testando a API da PetSchool com Insomnia

URL: [https://insomnia.rest/](https://insomnia.rest/)
---
Nesta atividade, vamos enviar requisições diretamente ao JSON Server. Assim, podemos observar o que a API recebe e devolve antes de comparar esse fluxo com as chamadas `fetch()` da aplicação Vue.

## Objetivos

Ao final, você deverá conseguir:

- identificar a URL, o método HTTP e o corpo de uma requisição;
- consultar todos os pets e um pet pelo ID;
- cadastrar um pet associado a um tutor;
- localizar o novo registro no `db.json`;
- relacionar uma requisição feita no Insomnia com o código `fetch()` da aplicação.

## 1. Iniciar o JSON Server

Abra um terminal na raiz do projeto, onde está o arquivo `db.json`, e execute:

```bash
npx json-server db.json
```

A API estará disponível em `http://localhost:3000`. Mantenha esse terminal aberto durante os testes.

> Se o projeto tiver um script `api` no `package.json`, você também poderá iniciar a API com `npm run api-fake`.

## 2. Criar uma coleção no Insomnia

Crie uma coleção chamada **PetSchool**. Dentro dela, adicione as requisições descritas nas próximas etapas.

Em cada resposta, observe:

1. o **status HTTP**;
2. o **corpo da resposta**;
3. a relação entre os dados recebidos e o conteúdo de `db.json`.

## 3. Listar os tutores

Crie e envie esta requisição:

```http
GET http://localhost:3000/tutores
```

**Resultado esperado:** uma lista de tutores. Cada tutor possui um `id`, que poderá ser usado como `tutorId` no cadastro de um pet.

**Pergunta:** qual é o `id` da Ana no seu `db.json`?

## 4. Listar os pets

Crie e envie:

```http
GET http://localhost:3000/pets
```

**Resultado esperado:** uma lista de pets com campos como `id`, `nome`, `especie` e `tutorId`.

O campo `tutorId` indica a qual tutor o pet está associado. Compare seu valor com os IDs encontrados na requisição anterior.

## 5. Consultar um pet pelo ID

Crie e envie:

```http
GET http://localhost:3000/pets/1
```

**Resultado esperado:** apenas o pet cujo `id` é `"1"`.

Agora substitua `1` por um ID que não existe:

```http
GET http://localhost:3000/pets/id-inexistente
```

Observe o **status HTTP**. É esse tipo de resposta que a aplicação Vue verifica com `resposta.ok` antes de tentar apresentar os dados.

## 6. Cadastrar um pet

Crie uma requisição `POST` para:

```http
POST http://localhost:3000/pets
```

No Insomnia, selecione **Body → JSON** e informe:

```json
{
  "nome": "Pipoca",
  "especie": "Gato",
  "tutorId": "1"
}
```

Envie a requisição e observe a resposta. O JSON Server acrescenta um `id` ao pet criado.

> Não é necessário enviar o `id` no corpo deste exemplo: deixe que o JSON Server o gere.

### O que significa `tutorId: "1"`?

Significa que Pipoca está associado ao tutor cujo `id` é `"1"`. O nome do tutor é apresentado pela aplicação Vue depois que ela consulta a coleção `/tutores` e encontra o registro correspondente.

## 7. Conferir o cadastro

Envie novamente:

```http
GET http://localhost:3000/pets
```

Procure Pipoca na lista. Depois abra o `db.json` e encontre o registro criado, incluindo seu novo `id`.

**Atenção:** o JSON Server pode incluir um campo `$schema` no `db.json`. Esse campo não representa um pet nem um tutor; ele aponta para o esquema usado na edição do arquivo.

## 8. Comparar o Insomnia com o código Vue

No Insomnia, você informou o método, a URL e o JSON usando a interface. Na view `AddPetView.vue`, o JavaScript faz o mesmo com `fetch()`:

```js
const resposta = await fetch('http://localhost:3000/pets', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    nome: 'Pipoca',
    especie: 'Gato',
    tutorId: '1',
  }),
});
```

| No Insomnia | No código Vue |
| --- | --- |
| Método `POST` | `method: 'POST'` |
| URL da requisição | Primeiro argumento de `fetch()` |
| Body em formato JSON | `body: JSON.stringify(...)` |
| Tipo do conteúdo: JSON | Header `Content-Type: application/json` |
| Resposta e status HTTP | `resposta` e `resposta.ok` |

O Insomnia permite testar a API diretamente. A aplicação Vue envia essas requisições quando a pessoa interage com a interface.

## Desafio opcional: alterar e excluir

**Faça estes testes apenas depois de concluir o cadastro.** Use o `id` gerado para Pipoca, sem modificar os pets iniciais da atividade.

Para alterar sua espécie, crie uma requisição `PATCH`:

```http
PATCH http://localhost:3000/pets/ID-DA-PIPOCA
```

Body em JSON:

```json
{
  "especie": "Cachorro"
}
```

Consulte o mesmo ID com `GET` e confira a alteração. Depois, se quiser testar a exclusão:

```http
DELETE http://localhost:3000/pets/ID-DA-PIPOCA
```

Por fim, envie `GET /pets` e verifique que Pipoca não aparece mais na lista.

> `PATCH` e `DELETE` alteram o `db.json`. Por isso, use apenas o pet que você criou para o teste.

## Checklist

- [ ] Iniciei o JSON Server.
- [ ] Criei a coleção PetSchool no Insomnia.
- [ ] Consultei `/tutores` e `/pets`.
- [ ] Consultei um pet existente e um ID inexistente.
- [ ] Cadastrei um pet com `POST /pets`.
- [ ] Encontrei o novo pet em `GET /pets` e no `db.json`.
- [ ] Identifiquei onde método, URL e JSON aparecem no `fetch()` da view.
