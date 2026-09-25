### Preparar os dados e a API local

1. Criar `db.json` na raiz do projeto:

```json
{
  "tutores": [
    { "id": "1", "nome": "Ana", "telefone": "47999990000" },
    { "id": "2", "nome": "Carlos", "telefone": "47988880000" }
  ],
  "pets": [
    { "id": "1", "nome": "Safira", "especie": "Cachorro", "tutorId": "1" },
    { "id": "2", "nome": "Nino", "especie": "Gato", "tutorId": "1" },
    { "id": "3", "nome": "Gaya", "especie": "Cachorro", "tutorId": "2" }
  ]
}
```

2. Em um terminal, iniciar a aplicação Vue com o comando previsto no template. Em outro, iniciar a API:

```bash
npm install json-server
npx json-server db.json
```

3. Como boa prática se quiser, você pode criar um alias no package.json:

Dentro da sessão: scripts, adicione o script abaixo:

`"api": "npx json-server db.json"`

4. Ao rodar a aplicação no terminal:

> npm run api-local

Será servido a porta: http://localhost:3000/

```text
Index:
http://localhost:3000/

Static files:
Serving ./public directory if it exists

Endpoints:
http://localhost:3000/tutores
http://localhost:3000/pets

```

OBS: Verificar `http://localhost:3000/pets` e `http://localhost:3000/tutores`. Os IDs são strings neste exemplo; mantenha o mesmo tipo em `id` e `tutorId`.
