# 🔍 Desafio de Automação de API — QA  
Automação desenvolvida para avaliação técnica utilizando a API pública **Serverest**.  

---

# 🛠️ Tecnologias utilizadas

- **Node.js**  
- **Cypress**  
- **JavaScript**  
- **cypress-plugin-api** (visualização de request/response no runner)

---

# 📁 Estrutura do Projeto

desafio-api-QA/

├── cypress/

│ ├── e2e/

│ │ ├── login.cy.js

│ │ ├── usuarios.cy.js

│ ├── fixtures/

│ │ └── dados.json

│ ├── support/

│ │ ├── commands.js

│ │ └── e2e.js

├── cypress.config.js

├── package.json

└── README.md

# 📦 Instalação do projeto

1. Criar o package.json
npm init -y

2. Instalar o Cypress
npm install cypress

3. Instalar o plugin de API (opcional, mas usado no projeto)
npm install cypress-plugin-api

-  É necessário ativar no cypress.config.js
env: {
  requestMode: true
}

- E importar no cypress/support/e2e.js
import 'cypress-plugin-api'

4. Abrir o Cypress em modo interativo
npx cypress open

5. Rodar todos os testes no terminal
npx cypress run

6. Rodar testes no dashboard
npx cypress run --record --key 5c2802f1-6bba-4a9e-88c8-56363bfb146d
https://cloud.cypress.io/projects/772sbp/

# 🧪 Funcionalidades e Cenários Automatizados

A API utilizada:
🔗 https://serverest.dev


1️⃣ Login

✔ Login com sucesso

Valida:

Status 200

Mensagem de sucesso


✔ Login com senha inválida

Valida:

Status 401

Mensagem de erro da API


2️⃣ Usuários

✔ Criar novo usuário com sucesso

Gera e-mail único

Valida status 201 e ID gerado

✔ Criar usuário duplicado

Usuário criado previamente

Segunda tentativa retorna 400

Valida mensagem “Este email já está sendo usado”

Obs.:
Para evitar erros, foi implementado o comando criarUsuarioSeNaoExistir, que aceita 201 ou 400.