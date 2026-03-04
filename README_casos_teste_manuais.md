# Casos de Teste Manuais — Sistema Helpdesk

---

# Funcionalidade: Criação de ticket

---

## Cenário 1: Criar ticket com dados válidos

```gherkin
Dado que estou com os dados de ticket válidos
E o título é "Erro ao acessar o sistema"
E a descrição é "Ao tentar login, a tela fica em branco"
E a prioridade é "Baixa"

Quando eu solicitar a criação do ticket

Então o ticket deve ser criado com sucesso
E deve ser retornado um ID único do ticket
E o status inicial do ticket deve ser "Aberto"
E o ticket criado deve estar disponível para consulta pelo ID retornado
```

## Cenário 2: Criar ticket com dados válidos (prioridade Alta)

```gherkin
Dado que estou com os dados de ticket válidos
E o título é "Sistema indisponível"
E a descrição é "Aplicação retorna erro 500 ao abrir a página inicial"
E a prioridade é "Alta"

Quando eu solicitar a criação do ticket

Então o ticket deve ser criado com sucesso
E deve ser retornado um ID único do ticket
E o status inicial do ticket deve ser "Aberto"
```

## Cenário 3 — Não permitir criação sem título

```gherkin
Dado que estou com os dados de ticket
E o título está vazio
E a descrição é "Não consigo finalizar o chamado"
E a prioridade é "Média"

Quando eu solicitar a criação do ticket

Então o ticket não deve ser criado
E deve ser exibida uma mensagem de erro informando que "Título é obrigatório"
```

## Cenário 4 — Não permitir prioridade inválida

```gherkin
Dado que estou com os dados de ticket
E o título é "Dúvida sobre funcionalidade"
E a descrição é "Como altero a senha?"
E a prioridade é "Urgente"

Quando eu solicitar a criação do ticket

Então o ticket não deve ser criado
E deve ser exibida uma mensagem de erro informando que "Prioridade inválida"
E deve ser informado o conjunto de prioridades aceitas
```

## Cenário 5 — Não permitir criação sem descrição

```gherkin
Dado que estou com os dados de ticket
E o título é "Erro intermitente"
E a descrição está vazia
E a prioridade é "Baixa"

Quando eu solicitar a criação do ticket

Então o ticket não deve ser criado
E deve ser exibida uma mensagem de erro informando que "Descrição é obrigatória"
```

---

# Funcionalidade: Atualização de status do ticket

---


## Cenário 6 — Atualizar status de Aberto para Em andamento

```gherkin
Dado que o ticket "TCK-1001" existe
E o status atual é "Aberto"

Quando eu atualizar o status do ticket "TCK-1001" para "Em andamento"

Então o status do ticket "TCK-1001" deve ser atualizado com sucesso
E ao consultar o ticket "TCK-1001" o status deve ser "Em andamento"
```


## Cenário 7 — Atualizar status de Em andamento para Fechado

```gherkin
Dado que o ticket "TCK-1001" existe
E eu atualizei previamente o status do ticket "TCK-1001" para "Em andamento"

Quando eu atualizar o status do ticket "TCK-1001" para "Fechado"

Então o status do ticket "TCK-1001" deve ser atualizado com sucesso
E ao consultar o ticket "TCK-1001" o status deve ser "Fechado"
```

## Cenário 8 — Não permitir atualizar ticket inexistente

```gherkin
Dado que não existe ticket com ID "TCK-9999"

Quando eu atualizar o status do ticket "TCK-9999" para "Em andamento"

Então o sistema deve retornar erro de "Ticket não encontrado"
E nenhum ticket deve ser alterado
```

## Cenário 9 — Não permitir status inválido

```gherkin
Dado que o ticket "TCK-1001" existe
E o status atual é "Aberto"

Quando eu atualizar o status do ticket "TCK-1001" para "Cancelado"

Então o sistema deve rejeitar a atualização
E deve ser exibida uma mensagem de erro informando que "Status inválido"
E ao consultar o ticket "TCK-1001" o status deve permanecer "Aberto"
```

## Cenário 10 — Não permitir status vazio

```gherkin
Dado que o ticket "TCK-1001" existe

Quando eu solicitar a atualização do status do ticket "TCK-1001" sem informar o novo status

Então o sistema deve rejeitar a atualização
E deve ser exibida uma mensagem de erro informando que "Status é obrigatório"
E ao consultar o ticket "TCK-1001" o status deve permanecer "Aberto"
```