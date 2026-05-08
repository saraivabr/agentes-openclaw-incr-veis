# Contribuindo para Agentes OpenClaw Incríveis

Agentes da comunidade são bem-vindos! Envie o seu e ganhe destaque em [crewclaw.com/agents](https://crewclaw.com/agents?utm_source=github&utm_medium=contributing&utm_campaign=submit).

---

## Sistema de Arquivos do Agente

Um agente é mais do que um prompt. É um sistema operacional completo.

```
agents/[categoria]/[nome-do-agente]/
├── SOUL.md          ← Identidade e personalidade (obrigatório)
├── README.md        ← Descrição e casos de uso (obrigatório)
├── AGENTS.md        ← Regras operacionais e instruções (opcional)
├── HEARTBEAT.md     ← Checklist de ativação (opcional)
└── WORKING.md       ← Template de tarefa inicial (opcional)
```

**SOUL.md** e **README.md** são obrigatórios. Os demais são opcionais, mas deixam seu agente pronto para produção.

---

## Envie seu Agente

### Opção 1: Pull Request (recomendado)

**Passo 1:** Fork e clone

```bash
git clone https://github.com/SEU-USUARIO/awesome-openclaw-agents.git
cd awesome-openclaw-agents
```

**Passo 2:** Crie a pasta do seu agente

```bash
mkdir -p agents/[categoria]/[nome-do-agente]
```

Categorias: `business`, `creative`, `data`, `development`, `devops`, `ecommerce`, `education`, `finance`, `freelance`, `healthcare`, `hr`, `legal`, `marketing`, `personal`, `productivity`, `real-estate`, `saas`, `security`

**Passo 3:** Escreva seu SOUL.md (obrigatório)

Quem é esse agente? Qual é a personalidade dele?

```markdown
# Nome do Agente

Breve descrição do agente.

## Identidade Principal

- **Papel:** O que o agente faz
- **Personalidade:** Como ele se comporta
- **Comunicação:** Como ele fala

## Responsabilidades

1. **Tarefa Principal**
   - Detalhe 1
   - Detalhe 2

## Diretrizes de Comportamento

### Deve:
- Comportamento positivo 1

### Não deve:
- Comportamento negativo 1

## Exemplos de Interação

**Usuário:** Exemplo de prompt
**Agente:** Exemplo de resposta
```

**Passo 4:** Escreva seu README.md (obrigatório)

```markdown
# Nome do Agente

> Descrição em uma linha

## Visão Geral

O que este agente faz e por que é útil.

## Casos de Uso

| Solicitação | Resultado   |
|-------------|-------------|
| Exemplo 1   | Resultado 1 |

## Arquivos

| Arquivo      | Finalidade                  |
|--------------|-----------------------------|
| SOUL.md      | Identidade e personalidade  |
| AGENTS.md    | Regras operacionais         |
| HEARTBEAT.md | Checklist de ativação       |
| WORKING.md   | Tarefa inicial              |

## Autor

Criado por [@seu-usuario](https://github.com/seu-usuario)
```

**Passo 5:** Adicione AGENTS.md (opcional)

Como o agente deve operar? Quais são as regras?

```markdown
# AGENTS.md — Regras Operacionais

## Espaço de Trabalho
- Ler/escrever arquivos no diretório de trabalho
- Armazenar descobertas na pasta memory/
- Registrar atividade diária em memory/AAAA-MM-DD.md

## Comunicação
- Publicar atualizações nas threads de tarefas
- Usar @menções para notificar outros agentes
- Manter mensagens concisas e acionáveis

## Ferramentas
- Sistema de arquivos: ler, escrever, pesquisar
- Shell: executar scripts, verificar logs
- Web: navegar, pesquisar, buscar dados

## Regras
- Sempre verificar WORKING.md na inicialização
- Atualizar WORKING.md após concluir uma tarefa
- Nunca tomar decisões fora do seu domínio
- Pedir esclarecimentos em vez de adivinhar
```

**Passo 6:** Adicione HEARTBEAT.md (opcional)

O que o agente deve verificar cada vez que for ativado?

```markdown
# HEARTBEAT.md — Checklist de Ativação

## Ao Ativar
- [ ] Ler WORKING.md para a tarefa atual
- [ ] Verificar @menções e notificações
- [ ] Revisar tarefas atribuídas

## Periódico
- [ ] Verificar feed de atividade para atualizações relevantes
- [ ] Checar se tarefas bloqueadas podem ser desbloqueadas
- [ ] Atualizar notas diárias em memory/

## Encerramento
- Se não houver tarefas nem menções, responder HEARTBEAT_OK
```

**Passo 7:** Adicione WORKING.md (opcional)

Qual é o estado inicial do agente?

```markdown
# WORKING.md — Estado Atual

## Tarefa Atual
Nenhuma tarefa ativa. Aguardando atribuição.

## Contexto
- Agente implantado e pronto
- Todas as integrações conectadas

## Próximos Passos
1. Verificar quadro de tarefas para novas atribuições
2. Revisar @menções pendentes
3. Iniciar trabalho no item de maior prioridade
```

**Passo 8:** Adicione entrada no `agents.json`

```json
{
  "id": "nome-do-seu-agente",
  "category": "categoria",
  "name": "Nome do Seu Agente",
  "role": "Descrição do papel em uma linha",
  "path": "agents/categoria/nome-do-seu-agente/SOUL.md",
  "deploy": "https://crewclaw.com/create-agent"
}
```

**Passo 9:** Envie o PR

```bash
git add .
git commit -m "Adiciona template de agente [NomeDoAgente]"
git push origin main
```

### Opção 2: Issue

Não quer configurar um PR? Use o template de issue **[Enviar Seu Agente](https://github.com/mergisi/awesome-openclaw-agents/issues/new?template=agent-submission.md)**. Cole seu SOUL.md e nós adicionamos para você.

---

## O Que Acontece Após o Merge

1. Seu agente aparece no [registro](https://github.com/mergisi/awesome-openclaw-agents/tree/main/agents)
2. Listado em [crewclaw.com/agents](https://crewclaw.com/agents?utm_source=github&utm_medium=contributing&utm_campaign=listed) com botão de implantação
3. Você é creditado como autor
4. A comunidade pode implantar seu agente com um clique

---

## Níveis de Envio

| Nível    | Arquivos                    | Badge                    |
|----------|-----------------------------|--------------------------|
| Básico   | SOUL.md + README.md         | Agente Comunitário       |
| Padrão   | + AGENTS.md                 | Agente de Produção       |
| Completo | + HEARTBEAT.md + WORKING.md | SO de Agente Completo    |

Envios completos ganham destaque no registro.

---

## Diretrizes de Estilo

- **Nomes de agentes:** Descritivos. `RevisorDeCodigo` e não `RC`
- **SOUL.md:** Cabeçalhos claros, exemplos de interação, diretrizes comportamentais específicas
- **AGENTS.md:** Regras concretas, não sugestões vagas
- **HEARTBEAT.md:** Checklist acionável, não prosa
- **README:** Comece com nome + uma linha descritiva, inclua tabela de casos de uso, credite o autor

---

## Checklist do PR

- [ ] SOUL.md segue o template acima
- [ ] README.md incluído
- [ ] Entrada adicionada ao `agents.json`
- [ ] Agente testado (funciona com OpenClaw ou framework similar)
- [ ] Sem links quebrados
- [ ] (Opcional) AGENTS.md, HEARTBEAT.md, WORKING.md incluídos

---

## Processo de Revisão

1. Mantenedor revisa em até 48 horas
2. Feedback se necessário
3. Merge e implantação em crewclaw.com/agents

Dúvidas? [Abra uma discussão](https://github.com/mergisi/awesome-openclaw-agents/discussions).
