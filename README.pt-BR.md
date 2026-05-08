# 🦞 Awesome OpenClaw Agents — Guia em Português (Brasil)

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Stars](https://img.shields.io/github/stars/mergisi/awesome-openclaw-agents?style=social)](https://github.com/mergisi/awesome-openclaw-agents)
[![Agents](https://img.shields.io/badge/agents-205-blueviolet)](agents/)

> Tradução e explicação da documentação principal do projeto para o público brasileiro.
>
> Este repositório reúne **205 templates de agentes de IA prontos para produção** no ecossistema OpenClaw. Cada template pode ser copiado, adaptado e usado como base para criar agentes com personalidade, regras, integrações e fluxo operacional definidos em `SOUL.md`.

**Idioma:** [English](README.md) · **Português (Brasil)**

---

## Visão geral

Se você está chegando agora ao OpenClaw, pense neste repositório como uma biblioteca prática de agentes prontos.

Em vez de começar do zero, você escolhe um template, copia o arquivo principal do agente e adapta ao seu cenário. Isso é útil para times, freelancers, operações internas, marketing, suporte, automações e dezenas de outros casos reais.

### O que você encontra aqui

- **Templates de agentes** organizados por categoria
- **Casos de uso reais** para entender o que outras pessoas já estão construindo
- **Skills reutilizáveis** para execução local
- **Quickstart** para subir o primeiro agente rapidamente
- **Configs de modelos** para migrar entre provedores
- **Padrão de memória** para reduzir custo e contexto
- **Guias de troubleshooting** para problemas comuns

---

## Índice

- [O que é um agente neste repositório?](#o-que-e-um-agente-neste-repositorio)
- [Catálogo de categorias](#catalogo-de-categorias)
- [Como começar rápido](#como-comecar-rapido)
- [Casos de uso reais](#casos-de-uso-reais)
- [Skills e subagentes](#skills-e-subagentes)
- [Por que OpenClaw?](#por-que-openclaw)
- [Deploy com CrewClaw](#deploy-com-crewclaw)
- [Servidores MCP e integrações](#servidores-mcp-e-integracoes)
- [Segurança](#seguranca)
- [Guias, custos e modelos](#guias-custos-e-modelos)
- [Memory Wiki](#memory-wiki)
- [Como contribuir](#como-contribuir)

---

<a id="o-que-e-um-agente-neste-repositorio"></a>

## O que é um agente neste repositório?

Aqui, um agente não é só um prompt solto.

Cada agente normalmente é organizado assim:

```text
agents/[categoria]/[nome-do-agente]/
├── SOUL.md      # identidade, papel, personalidade e regras principais
├── README.md    # descrição, contexto e casos de uso
├── AGENTS.md    # regras operacionais adicionais (opcional)
├── HEARTBEAT.md # checklist de ativação/rotina (opcional)
└── WORKING.md   # estado inicial ou tarefa atual (opcional)
```

### Como interpretar isso no contexto brasileiro

Para o público brasileiro, a lógica prática é simples:

- `SOUL.md` define **quem é o agente e como ele trabalha**
- `README.md` explica **quando usar e que tipo de problema ele resolve**
- `AGENTS.md`, `HEARTBEAT.md` e `WORKING.md` ajudam a transformar um template em algo mais próximo de operação contínua

Na prática, isso facilita criar agentes para:

- atendimento em WhatsApp
- automação de marketing
- análise financeira
- rotina de RH
- coordenação de tarefas de times
- monitoramento de infraestrutura
- produção de conteúdo

---

<a id="catalogo-de-categorias"></a>

## Catálogo de categorias

A documentação original lista **205 agentes em 24 categorias**. Abaixo está a tradução explicada das áreas principais, com links diretos para as pastas do repositório.

| Categoria | O que cobre | Pasta |
|---|---|---|
| Produtividade | priorização, relatórios, organização pessoal, reuniões | [agents/productivity/](agents/productivity/) |
| Desenvolvimento | revisão de código, testes, documentação, debugging, migrações | [agents/development/](agents/development/) |
| Marketing e Conteúdo | SEO, social media, newsletter, branding, outreach | [agents/marketing/](agents/marketing/) |
| Negócios | suporte, vendas, CRM, churn, faturamento | [agents/business/](agents/business/) |
| Pessoal | rotina, saúde, leitura, viagens, planejamento | [agents/personal/](agents/personal/) |
| DevOps | incidentes, deploy, observabilidade, capacidade, custos | [agents/devops/](agents/devops/) |
| Finanças | despesas, receita, previsões, fraudes, pagamentos | [agents/finance/](agents/finance/) |
| Educação | tutoria, quizzes, pesquisa, currículo, flashcards | [agents/education/](agents/education/) |
| Saúde | triagem, notas clínicas, alimentação, treino, bem-estar | [agents/healthcare/](agents/healthcare/) |
| Jurídico | contratos, políticas, conformidade, NDA, patentes | [agents/legal/](agents/legal/) |
| RH | recrutamento, onboarding, benefícios, avaliações | [agents/hr/](agents/hr/) |
| Criativo | branding, roteiro, copy, podcast, vídeo, UX | [agents/creative/](agents/creative/) |
| Segurança | vulnerabilidades, acessos, incidentes, phishing | [agents/security/](agents/security/) |
| E-commerce | catálogo, reviews, estoque, preços | [agents/ecommerce/](agents/ecommerce/) |
| Dados | entrada de dados, transcrição, pesquisa, análise | [agents/data/](agents/data/) |
| SaaS | operações e rotinas de produto para software | [agents/saas/](agents/saas/) |
| Imobiliário | prospecção, CRM e operações de real estate | [agents/real-estate/](agents/real-estate/) |
| Freelance | propostas, clientes, horas, entrega | [agents/freelance/](agents/freelance/) |
| Moltbook | presença social entre agentes | [agents/moltbook/](agents/moltbook/) |
| Supply Chain | rotas, previsão de estoque, avaliação de fornecedores | [agents/supply-chain/](agents/supply-chain/) |
| Compliance | GDPR, SOC 2, políticas de IA, risco | [agents/compliance/](agents/compliance/) |
| Voz | atendimento telefônico, voicemail, entrevistas | [agents/voice/](agents/voice/) |
| Customer Success | onboarding, NPS, retenção | [agents/customer-success/](agents/customer-success/) |
| Automação | rotinas recorrentes, briefing, candidaturas, negociação | [agents/automation/](agents/automation/) |

### Leitura prática para quem está no Brasil

Se você quer aplicar isso ao mercado brasileiro, alguns grupos tendem a ser os mais imediatos:

- **Business + Marketing** para times comerciais, suporte e aquisição
- **Development + DevOps** para squads de engenharia
- **Finance + Compliance** para backoffice e governança
- **Productivity + Automation** para operação individual e pequenas equipes
- **WhatsApp, atendimento e agenda** como frentes muito aderentes ao contexto local

---

<a id="como-comecar-rapido"></a>

## Como começar rápido

O projeto já traz um quickstart mínimo em Node.js para colocar um agente no ar em poucos minutos.

### Pré-requisitos

- [Node.js](https://nodejs.org/) 18+
- token de bot do Telegram
- chave de API da Anthropic ou OpenAI

### Passo a passo

```bash
git clone https://github.com/mergisi/awesome-openclaw-agents.git
cd awesome-openclaw-agents/quickstart
cp .env.example .env
npm install
cp ../agents/marketing/echo/SOUL.md ./SOUL.md
node bot.js
```

### Explicação em português

1. **Clona o repositório** e entra na pasta do exemplo mínimo.
2. **Configura o `.env`** com as credenciais necessárias.
3. **Escolhe um agente** copiando o `SOUL.md` de uma das categorias.
4. **Inicia o bot** e conversa com ele via Telegram.

Se quiser a documentação original desse fluxo, veja [quickstart/README.md](quickstart/README.md).

---

## Casos de uso reais

O repositório também mantém uma coleção de **132 casos de uso verificados** mostrando o que a comunidade realmente está construindo com OpenClaw.

Arquivo principal: [USE-CASES.md](USE-CASES.md)

### Alguns exemplos traduzidos

- coordenação de múltiplos agentes de desenvolvimento
- correção automática de erros e falhas de CI
- operação de caixa de e-mail com triagem e resposta
- automação de agenda, CRM e follow-up
- controle de casa inteligente com Home Assistant
- produção diária de conteúdo para redes sociais
- análise de SEO e outreach automatizado
- atendimento, prospecção e operações de negócios 24/7

### O que isso significa para o público brasileiro

Essa lista ajuda a sair do discurso genérico sobre IA e enxergar aplicações concretas para:

- pequenas empresas
- agências
- consultorias
- operações internas
- times de tecnologia
- creators e infoprodutores

---

## Skills e subagentes

Além dos templates principais, o repositório inclui recursos reutilizáveis.

### Skills

- [skills/gemma/](skills/gemma/) — skills para execução local com Gemma
- [skills/claude/](skills/claude/) — skills voltadas ao Claude Code
- [skills/README.md](skills/README.md) — visão geral das skills disponíveis

Exemplos de skills:

- explicação de código
- geração de mensagem de commit
- criação de resumo de reunião
- comparação de custo entre modelos
- diagnóstico de agente OpenClaw com falha

### Subagentes

A documentação original também apresenta subagentes especializados para Claude Code, como revisores de TypeScript, caçadores de falhas silenciosas, revisores de banco e analisadores de PR.

Na prática, isso permite delegar tarefas mais específicas sem perder o contexto do fluxo principal.

---

## Por que OpenClaw?

A proposta central do OpenClaw é ser **config-first**.

Em vez de exigir uma base grande de código ou um framework complexo, a ideia é:

1. escrever ou copiar um `SOUL.md`
2. registrar o agente
3. iniciar o gateway
4. colocar o agente para operar

### Resumo traduzido da comparação da documentação original

- contra frameworks de agentes, o OpenClaw tenta reduzir complexidade operacional
- contra alternativas ultraleves, ele oferece mais recursos, canais, orquestração e templates
- contra soluções empresariais hospedadas, ele preserva mais controle e self-hosting

### Em termos simples

Para quem está no Brasil e quer validar casos rápidos, o OpenClaw é interessante porque combina:

- entrada relativamente simples
- possibilidade de self-hosting
- integração com canais populares
- uso com provedores diferentes
- reaproveitamento de templates prontos

---

## Deploy com CrewClaw

A documentação original destaca o [CrewClaw](https://crewclaw.com/agents) como caminho para gerar um pacote completo de deploy com:

- `SOUL.md`
- `Dockerfile`
- `docker-compose.yml`
- bot pronto
- `.env.example`
- `package.json`
- `README.md`

### Leitura prática

Isso é útil para quem quer sair do modo “template de repositório” e ir para um pacote mais próximo de produção, sem montar tudo manualmente.

---

<a id="servidores-mcp-e-integracoes"></a>

## Servidores MCP e integrações

### MCP Servers

A documentação principal lista servidores MCP oficiais e comunitários para ampliar as capacidades dos agentes.

Exemplos:

- fetch/web browsing
- filesystem
- GitHub
- Slack
- Notion
- PostgreSQL
- Stripe
- Google Calendar

### Integrações

O repositório também destaca integrações e canais como:

- Telegram
- Slack
- Discord
- Email
- GitHub Actions
- n8n
- cron / pm2 / systemd

### Interpretação para o contexto brasileiro

Essas integrações tornam os agentes mais úteis em operações reais, especialmente quando conectados a:

- atendimento
- alertas internos
- rotinas de negócio
- calendário e mensagens
- automações recorrentes

---

<a id="seguranca"></a>

## Segurança

Arquivo recomendado: [TROUBLESHOOTING.md](TROUBLESHOOTING.md)

A documentação reforça que agentes OpenClaw rodam com acesso aos seus arquivos e serviços. Por isso, as práticas mínimas são:

- bind do gateway em `localhost`
- chaves em `.env`
- revisão de skills antes de instalar
- regras rígidas no `SOUL.md`
- limites de escopo, logs e budget caps

### Tradução do ponto principal

No contexto brasileiro, isso significa não tratar o agente como “bot mágico”. Ele deve ser operado com política de acesso, revisão e limites claros, especialmente em ambientes com dados de clientes, financeiro e operações.

---

## Guias, custos e modelos

### Guias e tutoriais

A README original reúne links para guias de:

- instalação e setup
- criação do primeiro agente
- uso com Ollama
- integração com Slack e Telegram
- multiagentes
- comparação com CrewAI, AutoGPT, LangChain e outros

### Custos e múltiplos provedores

O projeto também mantém uma seção sobre otimização de custo, com alternativas como:

- Claude Sonnet / Haiku
- GPT-4o Mini
- DeepSeek V3
- Ollama Cloud
- Ollama local

### Configs prontas

As pastas abaixo trazem bundles de configuração para migração entre modelos:

- [configs/glm-5.1/](configs/glm-5.1/)
- [configs/minimax-m2.7/](configs/minimax-m2.7/)
- [configs/gpt-5.4/](configs/gpt-5.4/)
- [configs/advisor-hybrid/](configs/advisor-hybrid/)
- [configs/ollama/](configs/ollama/)

### O valor disso para o público brasileiro

Como custo costuma ser uma variável decisiva por aqui, essa parte da documentação é especialmente relevante para:

- validar MVPs com menos gasto
- testar modelos locais
- reduzir dependência de um único provedor
- equilibrar qualidade, latência e preço

---

## Memory Wiki

Pasta principal: [memory-wiki/](memory-wiki/)

A proposta é usar memória pré-compilada em Markdown para reduzir custo de contexto e melhorar eficiência de sessão.

### Em linguagem simples

Em vez de o agente “explorar tudo” a cada execução, você entrega uma memória organizada e enxuta para ele consultar.

Isso é particularmente útil quando:

- o agente roda com frequência
- a operação tem contexto estável
- o custo de tokens importa
- você quer mais previsibilidade nas respostas

---

## Como contribuir

Guia completo: [CONTRIBUTING.md](CONTRIBUTING.md)

### Resumo traduzido

Para submeter um agente:

1. crie a pasta do agente dentro de `agents/[categoria]/[nome]`
2. adicione `SOUL.md` e `README.md`
3. opcionalmente inclua `AGENTS.md`, `HEARTBEAT.md` e `WORKING.md`
4. registre o agente em `agents.json`
5. abra um Pull Request

### Explicação para a comunidade brasileira

Se a ideia for popularizar o OpenClaw em português, uma boa contribuição é criar agentes adaptados a contextos locais, por exemplo:

- atendimento em português brasileiro
- fluxos de WhatsApp
- automação para pequenos negócios
- suporte para calendário, cobrança e vendas
- documentação mais acessível para iniciantes

---

## Leitura recomendada dentro do repositório

- [README.md](README.md) — versão original em inglês
- [quickstart/README.md](quickstart/README.md) — primeiro agente em execução
- [USE-CASES.md](USE-CASES.md) — catálogo de casos reais
- [TROUBLESHOOTING.md](TROUBLESHOOTING.md) — diagnóstico e recuperação
- [agents/README.md](agents/README.md) — visão geral dos agentes
- [skills/README.md](skills/README.md) — skills disponíveis

---

## Conclusão para o público brasileiro

Em termos práticos, este repositório é útil para quem quer **entender, testar e adaptar agentes de IA com menos esforço inicial**.

A melhor forma de usar este material no Brasil é:

- começar por um template próximo da sua operação
- rodar o quickstart
- traduzir e adaptar o `SOUL.md` para o seu contexto
- conectar o agente aos canais que sua equipe realmente usa
- evoluir com memória, integrações e regras mais rígidas

Se quiser consultar a documentação original completa, use [README.md](README.md).
