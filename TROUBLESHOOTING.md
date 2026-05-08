# Solução de Problemas do OpenClaw

Um guia consolidado para os problemas de estabilidade, custo e integração que mais aparecem no [r/openclaw](https://reddit.com/r/openclaw) e comunidades adjacentes. Este arquivo é mantido como referência da comunidade — cada entrada cita uma fonte para que você possa verificar os sintomas no seu ambiente antes de aplicar a solução.

**Versão estável atual:** `2026.4.11`
**Última revisão:** 2026-04-13

---

## Índice

- [Tabela de diagnóstico rápido](#tabela-de-diagnostico-rapido)
- [Problemas conhecidos por versão](#problemas-conhecidos-por-versao)
- [Custos excessivos](#custos-excessivos)
- [Bot parado / sem resposta](#bot-parado--sem-resposta)
- [Modos de falha do heartbeat](#modos-de-falha-do-heartbeat)
- [Problemas específicos por modelo](#problemas-especificos-por-modelo)
- [Escalação: quando pedir ajuda à comunidade](#escalacao-quando-pedir-ajuda-a-comunidade)
- [Contribuindo](#contribuindo)

---

<a id="tabela-de-diagnostico-rapido"></a>

## Tabela de diagnóstico rápido

Comece aqui. Encontre o sintoma, confirme a causa na seção indicada e aplique a solução.

| Sintoma | Causa mais provável | Solução | Seção |
|---|---|---|---|
| Bot responde algumas mensagens e fica em silêncio | Falha no heartbeat-model ou timeout do provedor | Reinicie o gateway, verifique a configuração do heartbeat | [Heartbeat](#modos-de-falha-do-heartbeat) |
| Custos subiram à noite sem mudança de tráfego | Regressão no TTL de cache do Claude (1h → 5m) OU bug de reset diário da `2026.4.8` | Fixe os headers de cache do Opus 4.6, atualize para `2026.4.11` | [Custos excessivos](#custos-excessivos) |
| Canal do Telegram em silêncio, sem erros no log | Regressão do Telegram na `2026.4.7` | Atualize para `2026.4.11` | [v2026.4.7](#v202647--telegram-fora-do-ar) |
| Conta API 3-5x acima do normal, `daily_budget` ignorado | Regressão de reset diário de sessão na `2026.4.8` | Atualize para `2026.4.11` ou corrija `gateway/budget.ts` | [v2026.4.8](#v202648--regressao-no-reset-diario) |
| `openclaw agent --status` retorna `UNKNOWN` | Corrupção do arquivo de sessões | Delete `~/.openclaw/agents/<nome>/sessions/sessions.json` | [Bot parado](#bot-parado--sem-resposta) |
| Chave da API do Claude banida apesar de pré-pago | Limite de burst disparou detecção de abuso | Contate suporte Anthropic, limite as tentativas de retry | [Claude Opus/Sonnet](#claude-opussonnet) |
| GPT 5.4 "parece lobotomizado" no OpenClaw | Problema de configuração, não do modelo | Defina `thinking=high` + `fastmode=true` | [GPT 5.4](#gpt-54) |
| Agente Minimax M2.7 recusa tarefas comerciais | Ressalva de licença comercial | Troque de provedor ou obtenha a licença | [Minimax M2.7](#minimax-m27) |
| Arquivos de memória crescem sem limite, latência aumenta | Inchaço de contexto | Compile a memória, remova entradas não utilizadas | [Inchaço de contexto](#inchaco-de-contexto-por-arquivos-de-memoria) |
| Contagem de tokens do Opus sobe sem novas tarefas | Loop descontrolado do executor advisor | Encerre o executor, verifique `last-plan.json` | [Loop do advisor](#loop-descontrolado-do-advisor) |
| Processo do bot ativo mas todos os agentes em `stale` | Thread de heartbeat do gateway morreu | Reinicie o gateway (`openclaw gateway restart`) | [Heartbeat](#modos-de-falha-do-heartbeat) |

---

<a id="problemas-conhecidos-por-versao"></a>

## Problemas conhecidos por versão

O OpenClaw lança atualizações semanalmente. O sentimento da comunidade, resumido por [um usuário](https://reddit.com/r/openclaw/comments/1sj9ich/), é que `2026.4.11` é "a primeira versão em muito tempo que não quebrou nada". Isso corresponde aos nossos testes — se você está em qualquer versão entre `2026.4.7` e `2026.4.10` e pode atualizar, atualize.

### v2026.4.11 (versão estável recomendada)

**Status:** Recomendada. Nenhuma regressão conhecida até 2026-04-13.

O que foi corrigido em relação à `4.10`:
- Reset diário de sessão respeita `daily_budget` novamente (corrige regressão da `4.8`).
- Handler do canal Telegram não descarta mais atualizações silenciosamente (corrige regressão da `4.7`).
- Thread de heartbeat reinicia em erro 5xx do provedor em vez de travar.

Fonte: [Is v.2026.4.11 the first version in a while that did not break things?](https://reddit.com/r/openclaw/comments/1sj9ich/)

### v2026.4.10 — sem problemas conhecidos (mas pule)

Nenhuma regressão confirmada, mas também não contém as correções da `4.11`. Pule direto para `4.11`.

### v2026.4.9 — sem problemas conhecidos (mas pule)

Mesma observação. O histórico de posts da comunidade não mostra reclamações específicas relacionadas à `4.9`, mas ela ainda carrega as regressões da `4.8` e `4.7`.

<a id="v202648--regressao-no-reset-diario"></a>

### v2026.4.8 — regressão no reset diário

**Sintoma:** Conta de API infla silenciosamente durante a noite. A configuração `daily_budget` parece ser ignorada. Os contadores de sessão são resetados com mais frequência do que uma vez por dia, portanto os limites de taxa nunca disparam.

**Impacto:** Multiplicadores de custo noturno de 3-5x relatados. Este é perigoso porque não há erros nos logs — o gateway continua rodando, mas reseta o contador de orçamento de forma muito agressiva.

**Detecção:** Compare a contagem de linhas de `~/.openclaw/metrics/daily.json` com os dias do calendário. Mais de uma linha por dia = você está afetado.

**Solução:** Atualize para `2026.4.11`. Se não puder atualizar, corrija `gateway/budget.ts` para reler o timestamp de reset do disco a cada tick em vez de armazená-lo em memória.

Fonte: [Regression in 2026.4.8 that silently breaks daily session reset and inflates your API bill](https://reddit.com/r/openclaw/comments/1shmg6l/)

<a id="v202647--telegram-fora-do-ar"></a>

### v2026.4.7 — Telegram fora do ar

**Sintoma:** O bot do Telegram aparece como conectado (`--status` retorna `OK`), mas as mensagens dos usuários nunca chegam aos agentes. As mensagens de saída dos agentes também falham, sem gerar erros.

**Impacto:** Para usuários que dependem do Telegram como interface mobile principal, isso parece que o bot está "morto" mesmo com o gateway saudável.

**Detecção:** Envie uma mensagem de teste conhecida para o seu bot e observe `~/.openclaw/gateway/logs/telegram.log`. Na `4.7` você verá o webhook sendo acionado, mas sem dispatch.

**Solução:** Atualize para `2026.4.11`. Fazer rollback para `2026.4.6` também funciona se não puder avançar.

Fonte: [OpenClaw 2026.4.7 Broke Telegram for Me](https://reddit.com/r/openclaw/comments/1sfh79p/)

### v2026.4.6 — última versão estável antes da janela `4.7`/`4.8`

Se precisar fazer rollback e não puder avançar, `4.6` é o alvo de rollback mais seguro. Sem problemas críticos conhecidos; faltam apenas os agentes multimídia lançados na `4.5`.

### v2026.4.5 — agentes multimídia introduzidos

Agentes `video_generate` e `music_generate` foram lançados aqui. Sem regressões conhecidas. Se você usa pacotes de implantação que referenciam agentes multimídia, esta é sua versão mínima.

### Versões anteriores

Não são ativamente rastreadas neste documento. Se você está em qualquer versão abaixo da `2026.4.5` e está tendo problemas, atualize primeiro e depois rediagnostique.

---

<a id="custos-excessivos"></a>

## Custos excessivos

Três causas explicam quase todos os posts "por que minha conta explodiu" no último mês. Verifique nesta ordem.

### Regressão no TTL do cache do Claude (1h → 5m)

**O que aconteceu:** O TTL do cache de prompt da Anthropic regrediu silenciosamente de 1 hora para 5 minutos em algumas camadas de conta. Agentes de longa execução que dependiam de acertos de cache para estabilidade de custo começaram a reler o contexto completo a cada turno.

**Como parece:**
- Contagem de tokens de entrada por turno aproximadamente dobra sem mudança no código do agente.
- Contagem de tokens de leitura de cache colapsa para quase zero.
- Curva de custo por sessão passa de plana para linear por turno.

**Detecção:** Nos seus logs de uso, compare `cache_read_input_tokens` vs `input_tokens` nos últimos 14 dias. Se a proporção caiu em uma data específica, você está afetado.

**Mitigação:**
1. Fixe suas requisições Claude com blocos explícitos `cache_control: {"type": "ephemeral"}` no system prompt e nas definições de ferramentas. Não dependa de cache implícito.
2. Agrupe turnos para que chamadas sequenciais de ferramentas fiquem dentro de uma janela de 5 minutos — se não puder amortizar por 1 hora, amortize por 5 minutos.
3. Para agentes que ficam ociosos por mais de 5 minutos entre turnos, considere uma camada de modelo diferente onde o comportamento de cache seja estável.

Fonte: [Did they just find the issue with Claude? "Cache TTL silently regressed from 1h to 5m"](https://reddit.com/r/ClaudeAI/comments/1sjxrp1/)

<a id="loop-descontrolado-do-advisor"></a>

### Loop descontrolado do advisor

**O que acontece:** Se você usa o padrão advisor (Scout planeja, executor executa), um bug no executor pode fazê-lo consultar o Opus repetidamente para o mesmo plano. Cada loop queima tokens de entrada do Opus contra um plano inalterado. Usuários relataram gastos noturnos com Opus de 10-20x o normal.

**Detecção:** Observe a contagem de tokens de entrada do Opus por sessão. Se uma única execução `--from-plan` ultrapassar `2 * tamanho_do_plano`, você está em loop.

**Causas comuns:**
- O executor alucina que um passo falhou quando na verdade teve sucesso, e então replaneja.
- `last-plan.json` não foi sobrescrito entre execuções, então o executor continua carregando o plano antigo.
- Invocação fire-and-forget sem rastrear códigos de saída do `run.cjs`.

**Solução:**
1. Sempre verifique o status de saída do `run.cjs`. Não use fire-and-forget.
2. Antes de cada execução, copie explicitamente o plano pretendido: `cp last-plan-{config}-{track}.json last-plan.json`.
3. Adicione um limite máximo de chamadas ao Opus por sessão na configuração do executor (`max_opus_calls_per_run: 3` é um bom ponto de partida).

<a id="inchaco-de-contexto-por-arquivos-de-memoria"></a>

### Inchaço de contexto por arquivos de memória

**O que acontece:** Arquivos de memória crescem sem limite à medida que os agentes os complementam. Quando a memória ultrapassa cerca de 50 mil tokens, cada sessão paga para reler o arquivo completo mesmo que a maior parte seja irrelevante para a tarefa atual.

**Detecção:** `wc -l ~/.openclaw/agents/<nome>/memory/*.md` — se o total for acima de ~8000 linhas, você está pagando por isso em cada turno.

**Mitigação:**
1. Compile a memória em vez de explorá-la. Mantenha um arquivo de índice curto que aponta para arquivos de detalhes; carregue os arquivos de detalhes apenas quando uma tarefa explicitamente precisar deles.
2. Archive entradas de memória com mais de 30 dias em um armazenamento frio que os agentes não carregam por padrão.
3. Veja o padrão `memory-wiki/` no repositório principal para a abordagem "compilar, não explorar".

---

<a id="bot-parado--sem-resposta"></a>

## Bot parado / sem resposta

### Árvore de diagnóstico

Percorra a lista de cima para baixo. Pare na primeira etapa que revelar o problema.

1. **O processo do gateway está ativo?**
   `ps aux | grep openclaw-gateway` — se não houver processo, `openclaw gateway restart`.

2. **O heartbeat está habilitado e rodando?**
   `openclaw agent --agent <nome> --status` — procure por `heartbeat: ok`. Se `heartbeat: stale`, vá para [Heartbeat](#modos-de-falha-do-heartbeat).

3. **O provedor do modelo está acessível?**
   Verifique a página de status do seu provedor. Anthropic e OpenAI tiveram degradações de várias horas no último mês. Verifique antes de assumir que é o seu ambiente.

4. **As sessões estão corrompidas?**
   `cat ~/.openclaw/agents/<nome>/sessions/sessions.json | head` — se não for JSON válido, delete o arquivo. O OpenClaw o recriará na próxima execução.

5. **A chave de API ainda é válida?**
   Teste a chave diretamente com `curl` contra o provedor. Chaves banidas retornam `401` ou `403` sem mensagem útil do OpenClaw. Veja [Conta da API do Claude banida](#claude-opussonnet).

6. **O disco está cheio?**
   Métricas e logs de sessão podem preencher rapidamente uma VM pequena. `df -h` — se estiver acima de 95%, limpe `~/.openclaw/metrics/archive/`.

7. **A porta está ligada?**
   Padrão do gateway: 18789. `lsof -i :18789` — se nada estiver ouvindo, reinicie o gateway.

### Recuperação do "bot morreu em 4 de abril"

Um caso representativo da comunidade: processo do gateway ativo, todos os agentes em `stale`, sem erros nos logs, última mensagem bem-sucedida com data de 4 de abril. O usuário se recuperou rodando o gateway completo dentro do Claude Code como subprocesso, o que restaurou o estado após reinicialização.

**Passos de recuperação que funcionaram:**
1. Pare o gateway.
2. Faça backup de `~/.openclaw/agents/*/sessions/` em um diretório com timestamp.
3. Delete os arquivos de sessão (não as configurações dos agentes).
4. Reinicie o gateway.
5. Envie uma mensagem de teste para cada agente para reconstruir o estado de sessão.

Esta também é a sequência correta para qualquer sintoma "tudo parece bem mas nada responde" onde a árvore de diagnóstico acima não encontrou o problema.

Fonte: [My OpenClaw bot died on April 4. I got it back inside Claude Code.](https://reddit.com/r/openclaw/comments/1sjz8n1/)

---

<a id="modos-de-falha-do-heartbeat"></a>

## Modos de falha do heartbeat

### O que é o heartbeat

No OpenClaw, "heartbeat" é uma thread em segundo plano dentro do gateway que periodicamente envia um ping para o provedor de modelo de cada agente com uma requisição mínima. Ele serve a dois propósitos: manter o estado da sessão aquecido e detectar degradação do provedor antes que as requisições do usuário expirem. O modelo usado para esses pings é o "heartbeat-model".

Muitos posts recentes no r/openclaw são sobre encontrar o heartbeat-model certo. A tensão é: você quer algo barato (faz pings a cada 30-120 segundos), rápido (não deve adicionar latência) e estável (você não quer que o próprio heartbeat quebre).

Fonte: [The search for a new "heartbeat-model"](https://reddit.com/r/openclaw/comments/1sgk8nj/) e [For All Noobies - Heartbeat.MD](https://reddit.com/r/openclaw/comments/1sj9bzr/)

### Modos de falha comuns

| Modo | Sintoma | Causa raiz | Solução |
|---|---|---|---|
| Travamento do heartbeat | `--status` retorna `stale`, processo do gateway ainda rodando | Thread do heartbeat travada em resposta 5xx | Reinicie o gateway. Atualize para `2026.4.11` que adiciona reinício da thread em 5xx. |
| Custo crescente do heartbeat | Conta do heartbeat-model cresce linearmente | Heartbeat-model muito caro para o intervalo de ping | Troque para um modelo menor (camada Haiku) ou aumente o intervalo de ping para 300s. |
| Falsos negativos | Heartbeat reporta `ok` mas requisições reais falham | Heartbeat-model está em um provedor diferente do agent-model | Alinhe o provedor do heartbeat-model com o provedor principal do agente. |
| Spam do heartbeat | Provedor limita a taxa da sua conta | Intervalo de ping muito curto, sem jitter | Adicione jitter ao intervalo, nunca fique abaixo de 30s. |

### Escolhas recomendadas de heartbeat-model (2026-04-13)

- **Claude Haiku 4** — mais barato, mais estável, mesmo provedor da maioria dos agentes OpenClaw. Recomendação padrão.
- **GPT-4.1 nano** — bom se seus agentes principais estiverem na OpenAI.
- **Gemini Flash 2.5** — barato, mas provedor diferente da maioria das configurações; use apenas se seus agentes estiverem neste provedor.

Não use Opus, Sonnet ou GPT-5.x como heartbeat-model. Você vai se arrepender na conta.

### Verificações de sanidade na configuração do heartbeat

```yaml
heartbeat:
  enabled: true
  model: claude-haiku-4
  interval_seconds: 60
  jitter_seconds: 15
  max_consecutive_failures: 3
  on_failure: restart_thread   # era "wedge" em versões < 2026.4.11
```

`on_failure: restart_thread` só está disponível na `2026.4.11` e posteriores. Em versões anteriores, você deve reiniciar o gateway manualmente quando o heartbeat travar.

---

<a id="problemas-especificos-por-modelo"></a>

## Problemas específicos por modelo

### GPT 5.4

A versão curta, de um post muito votado: **muitos relatos de "GPT 5.4 é ruim no OpenClaw" são problemas de configuração, não do modelo.**

Correção mais comum:
- Defina `thinking=high` na configuração do agente. `thinking=low` entrega um modelo muito mais fraco do que os benchmarks que você viu.
- Defina `fastmode=true`. Paradoxalmente, isso reduz a latência sem perder qualidade para a maioria das cargas de trabalho de agentes.
- Não combine `thinking=high` com `temperature > 0.4` — as saídas ficam instáveis.

Se você aplicou os três e o modelo ainda parece fraco, então você tem um problema real com o modelo. Antes disso, assuma que é configuração.

Fonte: [A lot of the new "GPT 5.4 sucks in OpenClaw" posts are really config issues](https://reddit.com/r/openclaw/comments/1sgpg8b/)

### Claude Opus / Sonnet

Três problemas ativos para conhecer:

1. **Risco de banimento de conta com chamadas rápidas à API.** Um usuário relatou ter sua conta pré-paga banida após uma rajada de tentativas de retry. A detecção de abuso da Anthropic não distingue entre "usuário clicando em retry" e "script em loop". Se o OpenClaw retornar um erro, não tente mais de 3 vezes com backoff exponencial.
   Fonte: [Claude API account banned despite pay as you go setup](https://reddit.com/r/openclaw/comments/1sf7iac/)

2. **Bug de TTL do cache.** Veja [custos excessivos](#regressao-no-ttl-do-cache-do-claude-1h--5m). Este é o maior problema de custo ativo.

3. **Limites de tamanho de sessão.** Os posts sobre "Hello usa 4%" são reais — com system prompts grandes e definições de ferramentas, uma única mensagem pode consumir 4-6% da janela de contexto antes de você dizer qualquer coisa. Mantenha os system prompts enxutos e use prompt caching de forma agressiva.

### GLM-5.1

Geralmente estável. Limitação conhecida: o adaptador de provedor do OpenClaw ainda não suporta streaming de ferramentas no GLM-5.1, então agentes com muitas ferramentas vão parecer lentos. Se seu agente usa ferramentas intensivamente, prefira Claude ou GPT.

### Minimax M2.7

**Ressalva de licença comercial:** o checkpoint M2.7 que a maioria das pessoas baixa do hub de modelos é de uso não comercial apenas. Se você usa em um produto, precisa de uma licença comercial da Minimax. Isso não é um bug do OpenClaw — mas agentes usando M2.7 às vezes se recusarão a completar prompts com aparência comercial devido ao aviso de licença incorporado nos pesos.

Solução: obtenha a licença comercial ou troque para um modelo diferente em implantações comerciais.

---

<a id="escalacao-quando-pedir-ajuda-a-comunidade"></a>

## Escalação: quando pedir ajuda à comunidade

### Antes de publicar

Verifique, nesta ordem:
1. Este arquivo, procurando por versão ou sintoma correspondente.
2. O changelog do OpenClaw para sua versão em execução.
3. Posts recentes no [r/openclaw](https://reddit.com/r/openclaw) (últimos 7 dias) — seu problema pode já ter resposta.
4. Issues do GitHub no repositório principal do OpenClaw, filtradas pela sua tag de versão.

### Template de post para o r/openclaw

Copie este modelo ao publicar. Relatórios de bug incompletos são ignorados; os completos geralmente recebem solução em menos de 24 horas.

```
**Versão do OpenClaw:** 2026.4.X
**SO:** macOS 14.x / Ubuntu 22.04 / ...
**Modelo principal:** claude-opus-4.6 / gpt-5.4 / ...
**Heartbeat model:** claude-haiku-4 / nenhum
**Sintoma (uma frase):**
**Quando começou:** AAAA-MM-DD
**O que mudou antes:** atualização de X para Y / novo agente / nova chave / nada

**Passos para reproduzir:**
1.
2.
3.

**Esperado:**
**Obtido:**

**Logs (censurados):**
```
tail -100 ~/.openclaw/gateway/logs/gateway.log
```

**O que já tentei:**
- [ ] Reiniciei o gateway
- [ ] Limpei as sessões
- [ ] Verifiquei status do provedor
- [ ] Li o TROUBLESHOOTING.md
```

O checklist ao final economiza o tempo de todos. Se você já limpou as sessões, as pessoas não vão pedir para você limpar as sessões.

### O que não publicar

- "OpenClaw está quebrado" sem número de versão.
- Capturas de tela de popups de erro sem o contexto de log ao redor.
- "Alguém mais está vendo isso?" sem descrever o sintoma.

Esses são descartados porque não há nada acionável.

---

<a id="contribuindo"></a>

## Contribuindo

PRs para este arquivo são bem-vindos. Cada nova entrada de problema deve conter:

- **Sintoma** — o que o usuário vê, em uma frase.
- **Reprodução** — passos mínimos para reproduzir, ou "intermitente" se não conseguir reproduzir com confiança.
- **Solução** — ação concreta, não especulação.
- **Fonte** — permalink do Reddit, link de issue do GitHub ou tag de versão do OpenClaw. Entradas sem fonte não serão aprovadas. "Confie em mim" não é uma fonte.

Mantenha o tom calmo e neutro. Este arquivo é um guia operacional, não peça de marketing nem desabafo. Se uma entrada parecer qualquer um dos dois, será reescrita antes do merge.

Quando uma versão sair da janela de suporte (aproximadamente 8 semanas), mova sua entrada de [Problemas conhecidos por versão](#problemas-conhecidos-por-versao) para uma seção histórica no final, mas não a delete — usuários em versões antigas ainda precisam encontrá-la via busca.
