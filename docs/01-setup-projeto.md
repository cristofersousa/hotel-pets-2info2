## Pré-requisitos

Antes de iniciar, verifique se estão instalados:

- [Node.js](https://nodejs.org/);
- npm;
- [Git](https://git-scm.com/);
- [Visual Studio Code](https://code.visualstudio.com/), recomendado.

O projeto utiliza o Node.js `22.18.0`, definido no arquivo `.nvmrc`.

```bash
node --version
npm --version
git --version
```

Caso utilize NVM no Linux ou macOS:

```bash
nvm install
nvm use
```

No Windows com `nvm-windows`:

```powershell
nvm install 22.18.0
nvm use 22.18.0
```

## Instalação

Clone o repositório:

```bash
git clone URL-DO-REPOSITORIO
```

Acesse o diretório e instale as dependências:

```bash
cd nome-do-projeto
npm install
```

## Execução

Inicie o servidor de desenvolvimento:

```bash
npm run dev
```

O terminal apresentará o endereço local da aplicação, normalmente:

```text
http://localhost:5173
```

Outros comandos disponíveis:

```bash
npm run format
npm run lint
npm run build
npm run preview
```
