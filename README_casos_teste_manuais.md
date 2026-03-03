# Funcionalidade: Criação de ticket
Como usuário do sistema de Helpdesk
Quero criar tickets com título, descrição e prioridade
Para registrar e acompanhar solicitações

Cenário: Criar ticket com dados válidos (prioridade Baixa)

Dado que estou com os dados de ticket válidos
E o título é "Erro ao acessar o sistema"
E a descrição é "Ao tentar login, a tela fica em branco"
E a prioridade é "Baixa"

Quando eu solicitar a criação do ticket

Então o ticket deve ser criado com sucesso
E deve ser retornado um ID único do ticket
E o status inicial do ticket deve ser "Aberto"
E o ticket criado deve estar disponível para consulta pelo ID retornado
Cenário: Criar ticket com dados válidos (prioridade Alta)

Dado que estou com os dados de ticket válidos
E o título é "Sistema indisponível"
E a descrição é "Aplicação retorna erro 500 ao abrir a página inicial"
E a prioridade é "Alta"

Quando eu solicitar a criação do ticket

Então o ticket deve ser criado com sucesso
E deve ser retornado um ID único do ticket
E o status inicial do ticket deve ser "Aberto"
Cenário: Não permitir criação de ticket sem título

Dado que estou com os dados de ticket
E o título está vazio
E a descrição é "Não consigo finalizar o chamado"
E a prioridade é "Média"

Quando eu solicitar a criação do ticket

Então o ticket não deve ser criado
E deve ser exibida uma mensagem de erro informando que "Título é obrigatório"
Cenário: Não permitir criação de ticket com prioridade inválida

Dado que estou com os dados de ticket
E o título é "Dúvida sobre funcionalidade"
E a descrição é "Como altero a senha?"
E a prioridade é "Urgente"

Quando eu solicitar a criação do ticket

Então o ticket não deve ser criado
E deve ser exibida uma mensagem de erro informando que "Prioridade inválida"
E deve ser informado o conjunto de prioridades aceitas
Cenário: Não permitir criação de ticket sem descrição

Dado que estou com os dados de ticket
E o título é "Erro intermitente"
E a descrição está vazia
E a prioridade é "Baixa"

Quando eu solicitar a criação do ticket

Então o ticket não deve ser criado
E deve ser exibida uma mensagem de erro informando que "Descrição é obrigatória"


# Funcionalidade: Atualização de status do ticket
Como usuário do sistema de Helpdesk
Quero atualizar o status de um ticket
Para refletir o andamento do atendimento

Contexto:
Dado que existe um ticket criado com ID "TCK-1001"
E o status atual do ticket "TCK-1001" é "Aberto"

Cenário: Atualizar status de "Aberto" para "Em andamento"

Dado que o ticket "TCK-1001" existe
E o status atual é "Aberto"

Quando eu atualizar o status do ticket "TCK-1001" para "Em andamento"

Então o status do ticket "TCK-1001" deve ser atualizado com sucesso
E ao consultar o ticket "TCK-1001" o status deve ser "Em andamento"
Cenário: Atualizar status de "Em andamento" para "Fechado"

Dado que o ticket "TCK-1001" existe
E eu atualizei previamente o status do ticket "TCK-1001" para "Em andamento"

Quando eu atualizar o status do ticket "TCK-1001" para "Fechado"

Então o status do ticket "TCK-1001" deve ser atualizado com sucesso
E ao consultar o ticket "TCK-1001" o status deve ser "Fechado"
Cenário: Não permitir atualizar status de ticket inexistente

Dado que não existe ticket com ID "TCK-9999"

Quando eu atualizar o status do ticket "TCK-9999" para "Em andamento"

Então o sistema deve retornar erro de "Ticket não encontrado"
E nenhum ticket deve ser alterado
Cenário: Não permitir atualizar status com valor inválido

Dado que o ticket "TCK-1001" existe
E o status atual é "Aberto"

Quando eu atualizar o status do ticket "TCK-1001" para "Cancelado"

Então o sistema deve rejeitar a atualização
E deve ser exibida uma mensagem de erro informando que "Status inválido"
E ao consultar o ticket "TCK-1001" o status deve permanecer "Aberto"
Cenário: Não permitir atualizar status sem informar o novo status

Dado que o ticket "TCK-1001" existe

Quando eu solicitar a atualização do status do ticket "TCK-1001" sem informar o novo status

Então o sistema deve rejeitar a atualização
E deve ser exibida uma mensagem de erro informando que "Status é obrigatório"
E ao consultar o ticket "TCK-1001" o status deve permanecer "Aberto"