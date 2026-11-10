# Hotel Pets — 2INFO2 🐾

Aplicação didática para gerenciamento de pets e tutores, desenvolvida nas aulas de **Desenvolvimento Web II**, na turma **2INFO2** do **Instituto Federal Catarinense — Campus Araquari**, com orientação do **Prof. Cristofer Sousa**.

O projeto trabalha componentes Vue, formulários, navegação entre páginas e consumo de uma API REST com JSON Server.

## Acesse o projeto

| Recurso | Endereço |
| --- | --- |
| Aplicação — Surge | [Hotel Pets](https://hotel-pets-ifc.surge.sh/pets) |
| API — Render | [API Hotel Pets](https://hotel-pets-2info2.onrender.com) |
| Dados de pets | [/pets](https://hotel-pets-2info2.onrender.com/pets) |
| Dados de tutores | [/tutores](https://hotel-pets-2info2.onrender.com/tutores) |

## Funcionalidades

- Listagem, cadastro e consulta de detalhes de pets e tutores.
- Associação de pets aos seus tutores.
- Atualização de dados com `PATCH`.
- Exclusão de pets e tutores.
- Navegação entre telas com Vue Router.

## Tecnologias

- Vue.js 3 e Vite.
- Vue Router.
- Bootstrap 5 e Bootstrap Icons.
- Fetch API para requisições HTTP.
- JSON Server para a API REST.
- Surge para a interface e Render para a API.

## Executar localmente

### Pré-requisitos

Instale [Git](https://git-scm.com/) e [Node.js](https://nodejs.org/) com npm. A versão indicada em `.nvmrc` é `22.18.0`.

Se utilizar NVM no Linux ou macOS, execute na pasta do projeto:

```bash
nvm install
nvm use
```

### Instalação

```bash
git clone https://github.com/cristofersousa/hotel-pets-2info2.git
cd hotel-pets-2info2
npm install
```

### Iniciar a API

Em um terminal:

```bash
npm run api
```

A API fica disponível em `http://localhost:3000`, utilizando o arquivo `db.json`.

### Iniciar a interface

Em outro terminal, na mesma pasta:

```bash
npm run dev
```

Acesse o endereço apresentado pelo Vite, normalmente `http://localhost:5173`. Mantenha os dois terminais abertos para utilizar a aplicação com a API local.

## Configurar a API por ambiente

Para utilizar a API local durante o desenvolvimento e a API do Render no site publicado, aplique esta configuração às chamadas:

```js
const API_URL = import.meta.env.DEV
  ? 'http://localhost:3000'
  : 'https://hotel-pets-2info2.onrender.com';

const resposta = await fetch(`${API_URL}/pets`);
```

Confira todas as requisições, inclusive as que ainda tenham o endereço escrito diretamente no `fetch`.

| Execução | API escolhida com essa configuração |
| --- | --- |
| `npm run dev` | JSON Server local |
| `npm run build` | Render |
| `npm run preview` | Render, ao visualizar o build gerado |

A escolha depende do ambiente. Não há troca automática para o Render caso a API local esteja desligada.

## Rotas da aplicação

| Rota | Tela |
| --- | --- |
| `/` | Redireciona para `/pets` |
| `/pets` | Listagem de pets |
| `/pets/novo` | Cadastro de pet |
| `/pets/:id` | Detalhes de um pet |
| `/tutores` | Listagem de tutores |
| `/tutores/novo` | Cadastro de tutor |
| `/tutores/:id` | Detalhes de um tutor e seus pets |


## API REST

O JSON Server disponibiliza os recursos `pets` e `tutores`. Cada pet possui um `tutorId`, que identifica seu tutor.

| Método | Exemplo | Finalidade |
| --- | --- | --- |
| `GET` | `/pets` | Listar pets |
| `GET` | `/pets/1` | Consultar pelo ID |
| `POST` | `/pets` | Cadastrar um registro |
| `PUT` | `/pets/1` | Substituir um registro |
| `PATCH` | `/pets/1` | Atualizar parte dos dados |
| `DELETE` | `/pets/1` | Excluir um registro |
| `GET` | `/tutores` | Listar tutores |
| `GET` | `/tutores/1` | Consultar pelo ID |
| `POST` | `/tutores` | Cadastrar um registro |
| `PUT` | `/tutores/1` | Substituir um registro |
| `PATCH` | `/tutores/1` | Atualizar parte dos dados |
| `DELETE` | `/tutores/1` | Excluir um registro |

Os mesmos padrões se aplicam a `/tutores`. A interface atual utiliza `PATCH` nas atualizações; o `PUT` também pode ser explorado diretamente na API para comparar os comportamentos.

## Publicar no Surge

Após conferir os endereços das requisições, gere o build:

```bash
npm run build
```

No Linux ou macOS:

```bash
cp dist/index.html dist/200.html
npx surge ./dist hotel-pets-ifc.surge.sh
```

No PowerShell, substitua o comando de cópia por:

```powershell
Copy-Item dist/index.html dist/200.html
```

O `200.html` permite abrir diretamente as rotas do Vue Router, como `/pets/1`. Para publicar no domínio existente, utilize uma conta com acesso a ele. Em uma cópia do projeto, escolha outro domínio.

Sempre que alterar a interface, gere novamente o build, copie o `index.html` para `200.html` e publique a pasta `dist`.

## Executar a API no Render

Crie um **Web Service** conectado ao repositório GitHub:

| Campo | Valor |
| --- | --- |
| Language | Node |
| Build Command | `npm install` |
| Start Command | `npx json-server db.json --host 0.0.0.0 --port $PORT` |

O Surge serve os arquivos da interface, enquanto o Render executa o JSON Server.

### Limitações da demonstração

No plano gratuito do Render, a API pode entrar em suspensão após 15 minutos sem acesso e levar cerca de um minuto para voltar. Alterações no `db.json` podem ser perdidas ao suspender, reiniciar ou republicar o serviço, pois o sistema de arquivos é temporário.

Os cadastros da API publicada são compartilhados entre os visitantes. Utilize dados fictícios nas atividades. Para armazenamento permanente, adote uma solução com persistência adequada.

Referência: [limitações dos serviços gratuitos do Render](https://render.com/docs/free).

## Comandos úteis

| Comando | Finalidade |
| --- | --- |
| `npm run dev` | Iniciar a interface em desenvolvimento |
| `npm run api` | Iniciar o JSON Server local |
| `npm run build` | Gerar os arquivos de publicação em `dist` |
| `npm run preview` | Visualizar localmente o build |
| `npm run format` | Formatar os arquivos de `src` |
| `npm run lint` | Executar os linters com correções automáticas |

## Objetivos de aprendizagem

- Trabalhar reatividade, componentes e formulários no Vue.
- Configurar navegação e rotas dinâmicas.
- Utilizar `async/await` e interpretar respostas JSON.
- Compreender métodos HTTP e operações de CRUD.
- Relacionar pets aos seus tutores.
- Diferenciar desenvolvimento local, build e publicação.

## Materiais das aulas

- [Testar o JSON Server no Insomnia](./docs/00-testar-json-server-insomnia.md)
- [Preparação do projeto](./docs/01-setup-projeto.md)
- [Configuração do projeto](./docs/02-configuracao-projeto.md)
- [Layout PetSchool](./docs/03-layout-petschool.md)
- [Criação das views](./docs/04-criar-views.md)
- [Listagem de pets](./docs/05-ListarPets.md)

## Professor e origem

**[Prof. Cristofer Sousa](https://github.com/cristofersousa)** — Desenvolvimento Web II, IFC — Campus Araquari.

Projeto iniciado a partir do [Template Vue.js com Bootstrap](https://github.com/ifc-dev-web2/tec-vue-bootstrap-template).
