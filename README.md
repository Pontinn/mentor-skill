# Professor — Claude Code Skill

Um modo de professor ativo para o Claude Code que acompanha seu projeto em tempo real. Nunca escreve código por você — ensina, desafia e te guia para encontrar as respostas sozinho.

---

## Instalação

1. Copie `SKILL.md` para `~/.claude/skills/professor/SKILL.md`
2. Reinicie o Claude Code
3. Execute `/professor` para começar

---

## Como Funciona

### Primeiro Uso (Perfil Global)

Na primeira chamada de `/professor` em qualquer projeto, um setup único de perfil é executado:

1. **Seleção de idioma** — primeira pergunta, sem texto antes. Suporta Português, Inglês, Espanhol, Francês, Alemão, Italiano, Japonês, Chinês, Coreano e mais.
2. **Perguntas de perfil** — campos obrigatórios (nome, linguagem foco, objetivo de carreira) seguidos de opcionais (data de nascimento, formação, estilo de aprendizado, área de interesse, maior dor).
3. **Pergunta da história** — opcional mas de alto impacto. Você descreve sua trajetória em tech em texto livre e/ou compartilha um link de portfólio/GitHub/LinkedIn. O professor lê, extrai contexto e preenche automaticamente os campos opcionais restantes com o que você compartilhou.
4. **Pergunta da maior dor** — explora bloqueios como síndrome do impostor, inconsistência nos estudos, insegurança, dependência de IA. Usado para calibrar tom e encorajamento em todas as sessões.

O perfil é salvo em `~/.claude/skills/professor/user_profile.md` e reutilizado em todas as sessões futuras. Nunca é perguntado novamente, a menos que você execute `/professor reset-profile`.

> **Detecção de aniversário:** se você informar sua data de nascimento, o professor te dará parabéns no início da sessão no dia do aniversário ou em até 7 dias após.

---

### Config por Projeto

Após o perfil global, cada projeto tem sua própria configuração única:

- Modo de ensino, idioma, comportamento do terminal, estilo de humor
- Tipo de projeto (aprendizado vs real/produção)
- Salvo em `.professor-config` na raiz do projeto (adicionado automaticamente ao `.gitignore`)

Nas sessões seguintes, a config é carregada silenciosamente e o professor vai direto para perguntar seu objetivo do dia.

Redefina com `/professor reset-project-config`.

---

## Invocação

```
/professor                          → exibe menu de config, depois inicia
/professor auto                     → define terminal=auto, pula menu, inicia
/professor misto ptbr auto ironico  → define todas as flags, pula menu, inicia
```

**Todos os argumentos apenas pré-configuram valores — nunca pulam as perguntas de inicialização.**

Switch de flag mid-session: `/professor auto` (ou qualquer flag) muda a configuração da sessão atual sem re-executar a inicialização.

---

## Modos de Ensino

| Modo | Comportamento |
|---|---|
| `questionar` | Nunca explica diretamente. Responde toda pergunta com uma pergunta. |
| `tutor` | Explica conceitos e teoria. Sem código. |
| `misto` _(padrão)_ | Explica teoria + guia com perguntas baseadas no contexto. |

---

## Estilos de Humor

`serio` `descolado` `ironico` `descolado+ironico` `pirata` `jedi` `coach` `filosofo` `drill` `hacker` `detetive` `rpg` `cientista` `comentarista` `poeta` `robo` `vilao` `vendedor` `shakespeariano`

Cada estilo de humor permeia completamente todas as respostas — elogios, correções, dicas e chamadas de atenção falam no personagem.

---

## Strict Mode

Ativado por padrão. Quando o professor detecta um problema real (durante a ronda ou revisão de código), ele:

1. Suspende o humor ativo: `Humor [nome] desativado.`
2. Entrega uma chamada de atenção direta e séria — mais dura se for **reincidência** (cruzado com o log de problemas da sessão)
3. Prefixa com empatia se frustração for detectada: _"Entendo sua frustração, mas..."_
4. Aguarda sua resposta antes de retomar o humor: `Humor [nome] ativado.`

Toggle com `/professor strict off` / `/professor strict on`.

---

## Comandos

| Comando | Descrição |
|---|---|
| `/professor hint` | Dica progressiva (3 níveis: leve → médio → forte) |
| `/professor reveal` | Solução completa com explicação detalhada |
| `/professor debate [tema] [modelo]` | Invoca um segundo professor para debater um tema |
| `/professor review` | Resumo da sessão: aprendido, pontos fracos, stats |
| `/professor quiz` | Perguntas rápidas de teoria sobre conceitos da sessão |
| `/professor concept [termo]` | Explicação aprofundada de um conceito específico |
| `/professor compare [A] vs [B]` | Comparação pedagógica lado a lado |
| `/professor pause` | Salva estado da sessão na memória |
| `/professor resume` | Carrega sessão pausada |
| `/professor progress` | Lista commits feitos nesta sessão |
| `/professor glossary` | Novos conceitos introduzidos na sessão |
| `/professor focus` | Desativa análise proativa temporariamente |
| `/professor focus off` | Reativa análise proativa |
| `/professor goal [obj] [prazo]` | Define uma meta de aprendizado com prazo |
| `/professor resource [tema]` | Sugestões de estudo (sem URLs) |
| `/professor quick-question [p]` | Resposta rápida sem perder o contexto atual |
| `/professor re-explain` | Reexplica o último conceito de outro ângulo |
| `/professor antipattern` | Antipatterns relevantes ao contexto atual do projeto |
| `/professor achievements` | Lista conquistas acumuladas nas sessões |
| `/professor history` | Resumo de sessões anteriores da memória |
| `/professor patrol [5\|10\|15\|off]` | Monitoramento periódico de código (padrão: off) |
| `/professor challenge [nível]` | Desafio de código integrado (básico/intermediário/avançado/expert) |
| `/professor strict [on\|off]` | Toggle de chamadas de atenção severas (padrão: on) |
| `/professor reset-project-config` | Apaga config do projeto e reinicia configuração |
| `/professor reset-profile` | Apaga perfil global e reinicia onboarding |

Todos os comandos também aceitam linguagem natural: _"me dá uma dica"_, _"quero ver a resposta"_, _"ativa o ronda"_, etc.

---

## Modo Ronda (Patrol)

`/professor patrol [5|10|15]` ativa monitoramento periódico via wake-ups agendados.

A cada gatilho:
- Executa `git diff HEAD`
- Silencioso se não houver mudanças
- Posta uma observação pedagógica breve se houver mudanças
- Aciona chamada de atenção do strict mode se um problema real for detectado

---

## Desafios de Código

`/professor challenge [nível]` inicia um desafio integrado na sessão do professor:

- Linguagem reutilizada da inicialização — nunca perguntada novamente
- Nível lembrado após o primeiro desafio da sessão
- Professor nunca escreve código de solução nem dá dicas algorítmicas
- `/ff` revela a solução completa com explicação
- Desafios concluídos são commitados e rastreados nas stats da sessão

---

## Nível Adaptativo

O professor monitora o desempenho ao longo das dicas usadas, reveals acionados e velocidade de resolução. Ajusta a complexidade silenciosamente ao longo do tempo e atualiza o nível na memória entre sessões.

---

## Memória e Persistência

| Arquivo | Propósito |
|---|---|
| `~/.claude/skills/professor/user_profile.md` | Perfil global do usuário (uma vez por usuário) |
| `[projeto]/.professor-config` | Config de sessão por projeto |
| `[projeto]/.claude/memory/professor_sessions.md` | Histórico de sessões e rastreamento de streak |
| `[projeto]/.claude/memory/professor_problem_log.md` | Log de problemas para detecção de reincidência no strict mode |

---

## Suporte a Plataformas

Detecta a plataforma em runtime:
- **Windows** → usa PowerShell para todas as operações de shell
- **macOS / Linux** → usa Bash para todas as operações de shell

Quando `terminal: auto` está ativo, `.claude/settings.json` é escrito na raiz do projeto **imediatamente** — antes de qualquer pergunta — para que nenhum prompt de permissão apareça durante a sessão.

---

## O Que o Professor Nunca Faz

- Escrever código funcional por você
- Dar a resposta antes de `/professor reveal` ou `/ff`
- Pular etapas de inicialização por causa de argumentos passados
- Expor labels internas de roteamento (PATH-A, PATH-B, STEP N) em qualquer mensagem
- Gerar URLs para recursos
