# AI Agents na Prática
## Do primeiro agente a sistemas agentic em produção

## Proposta do curso

Este curso ensina desenvolvimento de agentes de IA através da construção progressiva de um único projeto.

Em vez de estudar 18 conceitos isolados, o aluno começa com um agente extremamente simples e adiciona novas capacidades semana após semana:

**LLM → Tools → RAG → Guardrails → Planning → Multi-Agent → Context → Memory → Protocols → Browser → Deployment → Security**

O projeto condutor será um **AI Learning Agent** — um agente capaz de pesquisar conteúdos do curso, recomendar uma trilha de estudos, explicar conceitos, criar exercícios e acompanhar o contexto do aluno.

A cada módulo, a pergunta central será:

> **O que nosso agente consegue fazer agora que ele não conseguia fazer antes?**

---

## Público

Curso pensado inicialmente para desenvolvedores e profissionais técnicos que:

- sabem o básico de Python;
- já utilizaram algum LLM ou ChatGPT;
- entendem APIs em nível básico;
- ainda não construíram sistemas agentic completos.

Não assumimos experiência prévia com frameworks de agentes.

---

## Objetivo final

Ao concluir o curso, o aluno deverá ser capaz de projetar, implementar e avaliar um sistema de agentes que combine:

| Capacidade | O aluno deverá conseguir |
|---|---|
| Models | conectar e utilizar um LLM |
| Tools | permitir que o agente execute funções e APIs |
| RAG | fundamentar respostas em fontes externas |
| Planning | decompor objetivos em etapas |
| Context | controlar o que o modelo recebe |
| Memory | preservar informações úteis entre interações |
| Multi-Agent | distribuir responsabilidades entre agentes |
| Protocols | integrar ferramentas e agentes por padrões como MCP |
| Browser | executar tarefas em interfaces externas |
| Observability | entender o que aconteceu em cada execução |
| Evaluation | medir se o agente está funcionando corretamente |
| Deployment | levar o agente do notebook para produção |
| Security | controlar permissões, ações e riscos |

---

## Modelo mental que queremos construir

No começo do curso, é comum pensar:

> **“agente = chatbot que chama ferramentas.”**

No final, queremos que o aluno enxergue:

> **“agente = sistema de software probabilístico que combina modelos, ferramentas, conhecimento, contexto, estado, controle, avaliação e segurança para executar objetivos.”**

Essa mudança de modelo mental é um dos principais resultados de aprendizagem do curso.

---

# Arquitetura pedagógica

O curso segue os princípios da metodologia **AI Tutor**.

## Narrativa macro: Status Quo → Problema → Solução

Cada conceito começa mostrando o cenário atual, a limitação que aparece e a nova ideia que resolve essa limitação.

Exemplo para Tool Use:

**Status quo:** um LLM responde apenas usando o contexto disponível.

**Problema:** ele não consegue consultar nosso banco de dados, verificar um pedido ou executar uma ação.

**Solução:** oferecemos ferramentas que o modelo pode decidir utilizar.

Somente depois dessa intuição entramos na implementação.

## Narrativa da aula: What → Why → How

Dentro de cada aula usamos:

**What:** o que é?

**Why:** por que existe e quando faz diferença?

**How:** como funciona na prática?

A sequência evita começar mostrando APIs, classes e abstrações antes de o aluno entender o problema que elas resolvem.

## Princípios de ensino

1. **Plain language first.** Explique o conceito antes do jargão.
2. **Exemplos concretos.** Toda abstração deve ser ligada a um caso específico.
3. **Complexidade progressiva.** Primeiro intuição, depois detalhes, depois implementação.
4. **Uma história contínua.** O AI Learning Agent conecta todos os módulos.
5. **Aprender fazendo.** O aluno modifica, quebra, depura e melhora o sistema.
6. **Trade-offs explícitos.** O curso não apresenta arquiteturas como receitas universais.
7. **Segurança e avaliação recorrentes.** Esses temas aparecem desde as primeiras capacidades do agente.

---

# Projeto condutor: AI Learning Agent

O mesmo sistema evolui durante todo o curso.

## v0 — Conversational Agent

O aluno recebe uma solicitação como:

> “Quero aprender Tool Use.”

O agente responde com orientação simples, mas ainda não busca conteúdo, não usa ferramentas e não possui memória.

## v1 — Tool-enabled Agent

O agente ganha ferramentas como:

- `search_lessons()`
- `read_lesson()`
- `get_course_resources()`

Agora ele consegue agir sobre o repositório em vez de depender apenas do modelo.

## v2 — Grounded Agent

O agente passa a recuperar conteúdo das lições e fundamentar respostas nas fontes corretas.

Também introduzimos guardrails e decisões de confirmação humana.

## v3 — Planning Agent

O sistema transforma objetivos maiores em etapas, executa o plano e revisa o resultado antes de responder.

## v4 — Observable Agent

O aluno começa a registrar tool calls, fontes consultadas, latência, erros e sinais de qualidade.

## v5 — Context-Aware Agent

O agente diferencia claramente:

- prompt;
- contexto da interação;
- knowledge recuperado;
- memória persistida.

## v6 — Framework Agent

O aluno reorganiza conceitos já conhecidos usando abstrações do Microsoft Agent Framework.

## v7 — Environment Agent

O agente passa a interagir com ambientes externos, como browser/UI, com limites claros de aprovação humana.

## v8 — Production Agent

O sistema incorpora deployment, smoke tests, avaliação, observabilidade, segurança e decisões de escala.

---

# Jornada do curso

## Fase 0 — Preparando o laboratório

### Aula 00 — Course Setup

**Pergunta central:** Como vamos construir e experimentar agentes?

**Conteúdo do repositório:** `00-course-setup`

**Resultado:** ambiente funcionando, dependências instaladas, autenticação configurada e primeiro notebook executado.

**Entrega:** evidência de que o ambiente executa o notebook inicial sem erro de configuração.

---

# Fase 1 — Understand Agents

## Aula 01 — Intro to AI Agents

**Pergunta central:** O que transforma uma aplicação com LLM em um agente?

**Projeto:** AI Learning Agent v0.

**Resultado esperado:** o aluno consegue distinguir modelo, contexto, ferramentas e controle em um sistema agentic simples.

## Aula 02 — Agentic Frameworks

**Pergunta central:** Por que existem frameworks para agentes?

**Resultado esperado:** o aluno entende quais responsabilidades um framework pode assumir e quais decisões continuam sendo de arquitetura.

## Aula 03 — Agentic Design Patterns

**Pergunta central:** Como projetamos o comportamento de um agente antes do código?

**Resultado esperado:** o aluno consegue desenhar um fluxo agentic simples antes de escolher APIs ou classes.

---

# Fase 2 — Give Agents Capabilities

## Aula 04 — Tool Use

**Problema:** nosso agente conversa, mas não consegue agir fora do modelo.

**Upgrade:** AI Learning Agent v1.

**Resultado esperado:** o aluno cria pelo menos uma tool estreita, previsível e segura.

## Aula 05 — Agentic RAG

**Pergunta central:** Como fazer o agente responder com base nos documentos corretos em vez de depender da memória do modelo?

**Upgrade:** primeira parte do AI Learning Agent v2.

**Resultado esperado:** o agente recupera conteúdo relevante do curso e mostra de onde veio a resposta.

## Aula 06 — Building Trustworthy Agents

**Pergunta central:** Agora que o agente pode consultar dados e executar ações, como reduzimos comportamentos indesejados?

**Upgrade:** AI Learning Agent v2 completo.

**Resultado esperado:** o aluno adiciona ao menos uma regra de aprovação, limitação ou validação e consegue explicar o risco que ela reduz.

---

# Fase 3 — Orchestrate Intelligence

## Aula 07 — Planning Design

**Pergunta central:** Como transformar um objetivo maior em etapas verificáveis?

**Upgrade:** início do AI Learning Agent v3.

## Aula 08 — Multi-Agent Design

**Pergunta central:** Quando vale dividir uma tarefa entre agentes especializados?

**Regra didática:** multi-agent só aparece depois de o aluno enxergar claramente os limites de uma arquitetura com um único agente.

## Aula 09 — Metacognition

**Pergunta central:** Como adicionar uma etapa explícita de revisão sem tratar “auto-reflexão” como garantia de correção?

**Upgrade:** AI Learning Agent v3 completo.

---

# Fase 4 — Engineer Agent Systems

## Aula 10 — AI Agents in Production

**Pergunta central:** O que muda quando saímos do notebook?

**Tópicos:** qualidade, custo, latência, falhas, tracing e avaliação.

**Upgrade:** início do AI Learning Agent v4.

## Aula 11 — Agentic Protocols

**Pergunta central:** Como integrar agentes e ferramentas de forma mais padronizada?

**Tópicos:** protocolos de integração, incluindo MCP e A2A quando aplicável.

## Aula 12 — Context Engineering

**Pergunta central:** O modelo precisa realmente receber tudo?

**Resultado esperado:** o aluno consegue decidir o que deve entrar no próximo model call e o que deve permanecer fora.

## Aula 13 — Agent Memory

**Pergunta central:** O que vale a pena lembrar entre conversas?

**Upgrade:** AI Learning Agent v5.

**Checkpoint conceitual central:** o aluno deve conseguir diferenciar contexto, knowledge e memória sem recorrer apenas a definições decoradas.

---

# Fase 5 — Build in the Real World

## Aula 14 — Microsoft Agent Framework

**Pergunta central:** Como um framework organiza capacidades que já aprendemos manualmente?

**Regra didática:** primeiro o problema, depois o conceito, por último a API.

**Upgrade:** AI Learning Agent v6.

## Aula 15 — Computer Use / Browser Agents

**Pergunta central:** O que muda quando o agente precisa operar uma interface em vez de apenas chamar uma função?

**Upgrade:** início do AI Learning Agent v7.

**Prática obrigatória:** definir ao menos uma ação que exige confirmação humana antes da execução.

## Aula 16 — Deploying Scalable Agents

**Pergunta central:** Como transformar um protótipo funcional em um serviço confiável?

**Tópicos:** hosting, routing, caching, smoke tests, evaluation gates e observability.

**Upgrade:** início do AI Learning Agent v8.

## Aula 17 — Creating Local AI Agents

**Pergunta central:** Quais partes do agente podem ou devem permanecer locais?

**Tópicos:** privacidade, custo, conectividade, latência e limites de modelos locais.

---

# Fase 6 — Ship Safely

## Aula 18 — Securing AI Agents

**Pergunta central:** Como tornar ações agentic auditáveis, limitadas e mais difíceis de abusar?

**Tópicos:** least privilege, logging, receipts, permissões, ações sensíveis e superfícies de ataque.

**Upgrade:** AI Learning Agent v8 completo.

---

# Capstone — AI Learning Agent

O aluno deverá construir um agente capaz de receber algo como:

> “Quero aprender a construir agentes que usam ferramentas, mas ainda não entendo RAG.”

O sistema deverá:

1. entender o objetivo do aluno;
2. pesquisar o repositório;
3. selecionar conteúdos relevantes;
4. criar uma pequena trilha de estudos;
5. explicar os conceitos em linguagem acessível;
6. propor um exercício;
7. citar as fontes usadas;
8. registrar a execução de forma útil para debugging e avaliação.

## Arquitetura alvo de referência

```text
User
  ↓
Learning Agent
  ↓
Planner
  ↓
Course Search / RAG
  ↓
Tutor
  ↓
Self-check
  ↓
Resposta fundamentada
```

Essa arquitetura é apenas referência. O projeto não precisa necessariamente usar múltiplos agentes.

O aluno deverá justificar quando uma arquitetura com um único agente é mais simples e apropriada.

---

# Formato padrão de cada aula

Toda aula deve seguir esta sequência:

1. **Big Picture** — em uma frase, o que vamos aprender?
2. **Status Quo** — como resolveríamos o problema sem o conceito da aula?
3. **Problema** — onde essa abordagem quebra?
4. **Nova ideia** — apresentação do conceito em linguagem simples.
5. **What → Why → How** — aprofundamento progressivo.
6. **Exemplo concreto** — aplicação no AI Learning Agent.
7. **Código guiado** — leitura do notebook ou exemplo existente.
8. **Hands-on** — modificação feita pelo próprio aluno.
9. **Failure Mode** — fazer algo quebrar propositalmente e entender por quê.
10. **Checkpoint** — aluno explica o conceito com suas palavras.
11. **Upgrade do Agent** — incorporar a capacidade ao projeto contínuo.
12. **One-line takeaway** — resumo da aula em uma frase.

---

# Ritmo sugerido por aula

Para uma aula de aproximadamente 90 minutos:

| Etapa | Tempo |
|---|---:|
| Problema + intuição | 10 min |
| Conceito | 15 min |
| Exemplo | 10 min |
| Walkthrough do código | 15 min |
| Hands-on | 25 min |
| Debug / discussão | 10 min |
| Checkpoint | 5 min |

O foco recomendado é aproximadamente **40% conceito / 60% prática**.

---

# Avaliação

| Componente | Peso |
|---|---:|
| Exercícios das aulas | 20% |
| Checkpoints conceituais | 15% |
| Incrementos do projeto | 25% |
| Capstone | 30% |
| Explicação das decisões arquiteturais | 10% |

O aluno não deve ser avaliado apenas por “o código funciona”.

Uma parte importante da avaliação é conseguir explicar:

> **Por que essa arquitetura foi escolhida e quais são seus trade-offs?**

---

# Rubrica do Capstone

## Nível 1 — Functional

O agente responde e utiliza pelo menos uma ferramenta.

## Nível 2 — Grounded

Usa documentos externos e mostra suas fontes.

## Nível 3 — Agentic

Planeja e executa tarefas em múltiplas etapas quando necessário.

## Nível 4 — Reliable

Possui tratamento de erros, avaliação e observabilidade.

## Nível 5 — Production-ready

Possui decisões claras sobre segurança, memória, contexto, deployment e custo.

---

# Regra editorial central

O curso deve evitar começar explicações assim:

> “Microsoft Agent Framework possui a classe X que implementa Y...”

Preferimos começar assim:

> “Nosso agente agora precisa decidir qual ferramenta usar. Até este momento colocávamos essa lógica diretamente no código. Um framework nos permite representar esse comportamento de forma reutilizável.”

**Primeiro o problema. Depois o conceito. Por último a API.**

---

# Estrutura resumida da jornada

```text
Understand Agents
01–03
   ↓
Give Agents Capabilities
04–06
   ↓
Orchestrate Intelligence
07–09
   ↓
Engineer Agent Systems
10–13
   ↓
Build in the Real World
14–17
   ↓
Ship Safely
18
   ↓
Capstone
AI Learning Agent
```

---

# Critério de sucesso do curso

O curso foi bem-sucedido quando o aluno consegue:

- explicar o papel de cada componente do sistema;
- reconhecer quando uma arquitetura agentic é desnecessária;
- construir um agente que combina ferramentas, conhecimento e controle;
- medir e depurar seu comportamento;
- justificar decisões sobre contexto, memória, segurança e produção;
- evoluir de um notebook funcional para um sistema confiável e compreensível.
