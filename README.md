# 🤖 n8n Workflows — Agentes de IA e Automações de Negócio

> **15 projetos · 36 workflows · 555 nós**, construídos em **n8n** para negócios reais: agentes de IA no WhatsApp, CFO virtual, funis comerciais, geração de contratos, dashboards financeiros e observabilidade.

![n8n](https://img.shields.io/badge/n8n-EA4B71?style=flat&logo=n8n&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Google%20Gemini-8E75B2?style=flat&logo=googlegemini&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat&logo=openai&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=redis&logoColor=white)
![WhatsApp](https://img.shields.io/badge/WhatsApp-25D366?style=flat&logo=whatsapp&logoColor=white)
![Google Workspace](https://img.shields.io/badge/Google%20Workspace-4285F4?style=flat&logo=google&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)

---

## 📌 Sobre

Este repositório reúne automações que **resolveram problemas reais de negócio** em atendimento, finanças, marketing e comercial. Cada projeto tem o próprio README com arquitetura, nós usados e passo a passo de configuração.

Nenhum projeto é um exercício isolado. São soluções completas, de ponta a ponta: recebem a mensagem do cliente, entendem texto, áudio, imagem e PDF com IA, consultam dados do negócio, agem (registram, calculam, geram documento, agendam) e devolvem a resposta pelo canal certo.

---

## 🗂️ Catálogo de projetos

### 🎧 Atendimento ao cliente

| Projeto | O que resolve | Destaques |
|---|---|---|
| [**Agente de Atendimento — WhatsApp Oficial + Chatwoot**](./domains/customer-service/atendimentoViagens) | Atendimento conversacional com histórico no Chatwoot e follow-up automático de leads parados | API oficial da Meta, Chatwoot, agente Gemini com tool que registra o lead na planilha, follow-up agendado |
| [**Agente Conversacional Consultivo**](./domains/customer-service/darkay) | Qualifica leads e conduz até o agendamento de reunião com o time de vendas | Multimodal (texto, áudio, imagem, vídeo, documento), memória em Redis, aviso automático no grupo da equipe |
| [**Bot Comercial — Megahair**](./domains/customer-service/tabmegahair) | Atendente virtual "Paty" que segue um FAQ comercial e envia fotos do catálogo | Envio dinâmico de imagens por comprimento, 3 agentes, buffer de mensagens |
| [**Bot de Pedidos — Doceria**](./domains/customer-service/confeitariaV) | Apresenta o cardápio, coleta o pedido, calcula o valor e avisa a equipe | NocoDB como CRM, PDF do cardápio via Supabase, tool de calculadora, 2 agentes com memórias separadas |
| [**Gestão de Estoque via WhatsApp**](./domains/customer-service/aroma-cafe) | O gestor consulta e atualiza estoque e preços em linguagem natural | Google Sheets como base, whitelist de acesso, Gemini |

### 💰 Finanças

| Projeto | O que resolve | Destaques |
|---|---|---|
| [**CFO Digital — Agente Financeiro**](./domains/finance/srl-confeccoes) | CFO virtual: análises de DRE, fluxo de caixa e centros de custo pelo WhatsApp | Sub-agente de cálculos, relatório em **PDF** gerado na hora, **guardrails** anti-jailbreak |
| [**Automação Financeira — ContaAzul**](./domains/finance/lemon-consultoria) | Consolida dados financeiros diários e mensais e distribui por e-mail e SharePoint | OAuth2 com renovação automática de token, Microsoft Graph, Gmail |
| [**Dashboard DRE / Fluxo de Caixa**](./domains/finance/DRE-dashboard) | Sincroniza planilha → PostgreSQL e alimenta um dashboard HTML quase em tempo real | Google Apps Script, Postgres, dashboard próprio em HTML/JS |

### 🧑‍💼 Comercial e contratos

| Projeto | O que resolve | Destaques |
|---|---|---|
| [**Funil Comercial com Qualificação**](./domains/recruitment/funil_comercial) | Atendente "Gabi" qualifica leads em 7 etapas e bifurca pelo faturamento | Acima de R$ 10 mil/mês agenda reunião no Google Calendar; abaixo, oferece infoproduto; agente de pós-processamento extrai o lead em JSON |
| [**Contratos Automáticos**](./domains/recruitment/contratos_automaticos) | Gera o contrato a partir da planilha e envia para assinatura digital, sem toque humano | 3 modelos de contrato, Google Docs API → PDF → **Autentique** |

### 📣 Marketing

| Projeto | O que resolve | Destaques |
|---|---|---|
| [**Onboarding de Clientes — ClickUp**](./domains/marketing/integracao_clickup) | Detecta clientes novos no ClickUp e provisiona pasta no Drive e grupo no WhatsApp | Personalização por tipo de contrato, n8n Data Tables para controle de estado |

### 💻 TI e operações

| Projeto | O que resolve | Destaques |
|---|---|---|
| [**Notion Lesson Generator**](./domains/information-technology/Notion-Lesson-Generator) | Transforma um export `.zip` do Notion em material de aula personalizado | Drive Trigger, descompactação, Gemini gerando o conteúdo |
| [**Notificador de Erros**](./domains/general/n8n-observability) | Alerta no WhatsApp quando qualquer workflow falha, com link para a execução | Error Workflow reutilizável em toda a instância |
| [**Lixeira de Execuções**](./domains/general/execution-cleanup) | Limpa execuções antigas toda semana para manter a instância leve | API interna do n8n, agendamento semanal |
| [**Scraping de Cruzeiros**](./domains/general/web-scraping) | Coleta, normaliza e deduplica dados paginados de uma API | Paginação, normalização em Code node, deduplicação |

---

## 🧱 Padrões de arquitetura

Os agentes de WhatsApp compartilham uma arquitetura que foi amadurecendo entre os projetos. Nem todo projeto usa todas as camadas; o README de cada um mostra o fluxo exato.

```
 WhatsApp (Evolution API / UazAPI / Meta Oficial)
                 │  webhook
                 ▼
 ┌──────────────────────────────┐
 │  Buffer de mensagens (Redis) │  agrupa mensagens enviadas em sequência
 └──────────────┬───────────────┘  antes de chamar o modelo
                ▼
 ┌──────────────────────────────┐
 │  Normalização de mídia       │  áudio → transcrição · imagem/vídeo → descrição
 │  (Gemini multimodal)         │  PDF → extração de texto
 └──────────────┬───────────────┘
                ▼
 ┌──────────────────────────────┐
 │  Guardrails + whitelist      │  bloqueia jailbreak/NSFW e números não autorizados
 └──────────────┬───────────────┘
                ▼
 ┌──────────────────────────────┐      ┌───────────────────────────┐
 │  Agente de IA                │ ───▶ │  Tools                    │
 │  (prompt + memória Redis)    │      │  Sheets · NocoDB · Drive  │
 └──────────────┬───────────────┘      │  Calendar · sub-agentes   │
                ▼                      └───────────────────────────┘
 ┌──────────────────────────────┐
 │  Sub-workflows de envio      │  texto, áudio, imagem, PDF
 └──────────────┬───────────────┘
                ▼
     Notificação interna (grupo da equipe) + registro do lead
```

| Padrão | Por que importa |
|---|---|
| **Buffer de mensagens** | O usuário manda "oi", "tudo bem?", "quero um orçamento" em 3 mensagens. O buffer junta tudo e o agente responde uma vez, com contexto. |
| **Sub-workflows de envio** | Texto, áudio e mídia ficam isolados e reaproveitáveis. Trocar de provedor de WhatsApp muda um workflow, não todos. |
| **Memória em Redis** | Mantém o contexto da conversa por sessão, com baixa latência. |
| **Sub-agentes como tools** | Cálculos e consultas pesadas ficam em agentes especializados, e o prompt principal continua enxuto. |
| **Guardrails e whitelist** | Protegem dados financeiros e o custo de tokens contra uso indevido. |
| **Error Workflow central** | Qualquer falha em produção vira um alerta no WhatsApp em segundos. |

---

## ⚙️ Stack

| Categoria | Ferramentas |
|---|---|
| Orquestração | n8n (AI Agent, sub-workflows, Data Tables, Error Workflow) |
| LLMs | Google Gemini (multimodal), OpenAI |
| Mensageria | Evolution API, UazAPI, WhatsApp Cloud API (Meta), Chatwoot |
| Dados | Redis, PostgreSQL, Google Sheets, NocoDB, Supabase |
| Produtividade | Google Drive, Docs, Calendar, Gmail, ClickUp, Microsoft SharePoint (Graph) |
| Integrações | ContaAzul (OAuth2), Autentique, Google Apps Script, APIs REST |

---

## 🚀 Como usar

1. Escolha o projeto no catálogo acima e abra a pasta.
2. Baixe os arquivos `.json`. Importe primeiro os **sub-workflows** e depois o principal, porque o principal referencia os outros pelo ID.
3. No n8n: **Workflows → Import from File**.
4. Crie as credenciais pedidas (Gemini, Redis, Google, provedor de WhatsApp etc.).
5. Substitua os placeholders `{{...}}` (veja abaixo) e atualize os IDs dos sub-workflows nos nós **Execute Workflow**.
6. Teste com o **Test workflow** e só então ative.

O README de cada projeto lista as credenciais e as variáveis específicas dele.

---

## 🔐 Segurança e dados sensíveis

Todos os workflows foram **sanitizados** antes da publicação:

- Tokens, chaves de API, IDs de planilhas e pastas, domínios de instâncias e números de telefone foram trocados por placeholders como `{{META_WHATSAPP_ACCESS_TOKEN}}`, `{{GOOGLE_SHEET_ID}}` e `5511999999999`.
- Os dados de execução fixados (`pinData`) e o `instanceId` de origem foram removidos.
- As credenciais do n8n **nunca** são exportadas junto com o workflow: cada nó só referencia a credencial pelo nome.

> Se for publicar seus próprios workflows, faça o mesmo. Um export do n8n pode carregar tokens colados em headers HTTP e dados reais de clientes no `pinData`.

---

## 👥 Autores

Projeto desenvolvido em conjunto por:

- **Lucas Fugimura** — [@Fgmmra](https://github.com/Fgmmra) · [LinkedIn](https://www.linkedin.com/in/lucasfugimura)
- **Caio "Kayn" Silva** — [@KaynBelmont](https://github.com/KaynBelmont)
- **Gustavo Bispo Cordeiro** — [@bispado](https://github.com/bispado) · [LinkedIn](https://linkedin.com/in/gustavo-bispo-cordeiro-59533a207)

Repositório original: [Fgmmra/n8n-workflows](https://github.com/Fgmmra/n8n-workflows)
