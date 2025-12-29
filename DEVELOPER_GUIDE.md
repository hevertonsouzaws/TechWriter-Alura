# Guia do Desenvolvedor: Connect CRM

> **Ambiente:** Desenvolvimento (`app-dev`)  
> **Status:** Em construção  
> **Público-alvo:** Engenheiros de Software (Backend e Frontend)

## 1. Visão Geral Técnica
O **Connect CRM** é o módulo responsável pela gestão do funil de vendas, qualificação de leads (SDR) e automação de campanhas. Ele faz parte do ecossistema Websupply, mas possui regras de negócio específicas para o fluxo comercial.

---

## 2. Stack Tecnológica (A definir)
* **Frontend:**  Vue.js / TypeScript
* **Backend:** .NET 8
* **Banco de Dados:** SQL Server
  
---

## 3. Regras de Negócio no Código (Lógica do Sistema)

### 3.1 Restrição de Campanhas
Existe uma trava de segurança no Backend para a criação de campanhas:
* **Regra:** O sistema bloqueia a seleção de empresas que não possuam um **Canal Ativo** (e-mail ou telefone validado).
* **Nota de Implementação:** Requisições para disparar campanhas sem canais ativos retornarão erro `400 Bad Request`.

### 3.2 Conversão de Lead para Oportunidade
* Ao realizar a conversão, o `lead_id` deve ser persistido na tabela de `opportunities` para garantir o rastreio completo do histórico (Audit Trail).

---

## 4. Endpoints da API (Ambiente Dev)

| Método | Endpoint | Descrição |
| :--- | :--- | :--- |
| `GET` | `/api/v1/leads` | Retorna a listagem paginada de leads. |
| `POST` | `/api/v1/campaigns/trigger` | Aciona o worker de disparo de mensagens. |
| `PATCH` | `/api/v1/opportunities/{id}` | Atualiza a fase do funil (Card do Kanban). |

---

## 5. Configuração Local (Setup)

### 5.1 Variáveis de Ambiente
Crie um arquivo `.env` na raiz do projeto baseando-se no `.env.example`:
```env
DB_HOST=localhost
API_KEY_CONNECT=sua_chave_aqui
DEBUG_MODE=true
