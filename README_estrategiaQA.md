# Desafio QA — Sistema de Gestão de Tickets (Helpdesk)

## 1) Principais riscos de qualidade do sistema

### Regras de negócio e integridade dos dados
- **Campos obrigatórios**: criar ticket sem título/descrição/prioridade (ou com valores inválidos) e o sistema aceitar.
- **Prioridade inválida**: aceitar valores fora do domínio (ex.: “Urgente” se não existir, string vazia, null).
- **Status inválido / transições incorretas**: permitir status fora do conjunto {Aberto, Em andamento, Fechado} ou regressão indevida (ex.: Fechado → Em andamento) sem regra definida.
- **ID e consistência**: consulta por ID retornar ticket errado, duplicidade de IDs, ou comportamento inconsistente após atualização.

### Confiabilidade da API/serviço
- **Códigos HTTP incorretos** (ex.: retornar 200 para erro de validação; não usar 404 quando ID não existe).
- **Mensagens de erro pouco claras**: dificulta debugging e experiência do usuário.
- **Persistência**: ticket criado não aparecer na listagem, ou atualização não persistir.

### Segurança e exposição indevida
- **Injeção de conteúdo**: título/descrição com scripts/HTML causando XSS no front (se houver UI).
- **Enumeração de IDs**: se houver autenticação no futuro, risco de acesso a tickets de terceiros via tentativa de IDs (mesmo que hoje não exista login, é um risco evolutivo).

### Observabilidade e suporte
- **Logs insuficientes** para rastrear criação/alteração de status.
- **Sem rastreabilidade** de mudanças (quem/quando) — risco de auditoria e suporte (dependendo do objetivo do produto).

---

## 2) Tipos de testes essenciais para esse contexto

### Testes funcionais (principais fluxos)
- Criar ticket com dados válidos (título, descrição, prioridade).
- Listar tickets e validar que o criado aparece.
- Consultar ticket por ID (existente e inexistente).
- Atualizar status (para cada status permitido).

### Testes de validação (negativos)
- Criar ticket com:
  - título vazio/nulo / muito longo
  - descrição vazia/nula / muito longa
  - prioridade inválida (fora do domínio, tipo incorreto)
- Atualizar status com:
  - status inválido
  - ID inexistente
  - payload incompleto

### Testes de contrato de API
- Esquema do ticket: tipos corretos (id numérico/uuid, strings, enums).
- Campos obrigatórios presentes nas respostas.

### Testes de integração
- API ↔ camada de persistência (DB/arquivo/memória): criação/listagem/consulta/atualização realmente persistem.

### Testes de regressão
- Pacote enxuto cobrindo os endpoints e regras críticas para evitar quebra ao evoluir o sistema.

### Testes não funcionais (mínimo viável)
- **Performance básica**: listagem com volume (ex.: 1k–10k tickets) sem degradação anormal.
- **Confiabilidade**: comportamento sob requisições repetidas (idempotência onde aplicável).
- **Segurança básica**: sanitização/escape de saída (especialmente se houver UI), e validação de entrada.

---

## 3) O que eu priorizaria automatizar primeiro (e por quê)

### Prioridade 1 — fluxos de API
Automatizar o fluxo crítico que valida o sistema inteiro:
1. **Criar ticket** (retorna ID)
2. **Consultar por ID** (valida persistência e payload)
3. **Listar tickets** (valida visibilidade do ticket)
4. **Atualizar status** (valida regra e persistência)
5. **Consultar novamente** (valida atualização)

**Motivo:** cobre as funcionalidades principais com alto retorno, detecta falhas graves cedo, é rápido, determinístico e fácil de manter.

### Prioridade 2 — Validações e erros (reduz risco de dados ruins)
- Casos negativos mais prováveis: campos obrigatórios, enums inválidos, ID inexistente.
**Motivo:** impede lixo de dados, melhora confiabilidade e reduz incidentes em produção.

### Prioridade 3 — Contrato de resposta
- Validação de schema e tipos (ex.: JSON Schema/Pydantic validation).
**Motivo:** garante estabilidade do formato.

---

