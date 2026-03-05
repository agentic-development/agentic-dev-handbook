---
title: Pilares Centrais
description: >-
  Os quatro pilares arquitetônicos que permitem a entrega de software segura,
  escalável e assistida por IA
authors:
  - dpavancini
lastUpdated: '2026-02-24'
---

## Visão Geral

O Agente de Desenvolvimento (Agentic Development Framework) se apoia em quatro pilares arquitetônicos que trabalham juntos para permitir a entrega de software assistida por AI de forma segura e escalável. Cada pilar aborda uma dimensão diferente do desafio: o que os agents sabem, onde eles executam, como são governados e como o trabalho é roteado.

## Pilar 1: Arquitetura Context-First

[[context-engineering|Context Engineering]] é a base de todo o resto. No desenvolvimento agentic, o context é código — é a entrada primária que determina a qualidade, a correção e a velocidade de cada execução do agent.

### Clareza do Context como a Restrição Primária

O desenvolvimento tradicional é restrito pelo tempo do desenvolvedor. O desenvolvimento agentic é restrito pela clareza do context. Quando os agents possuem context preciso, completo e bem estruturado, eles executam de forma confiável e rápida. Quando o context é vago ou incompleto, eles produzem alucinações, perdem casos de borda ou entram em loops improdutivos.

Isso significa que a [[context-window]] não é apenas uma limitação técnica dos LLMs — é uma restrição arquitetônica que molda como você decompõe o trabalho, estrutura as especificações e organiza o conhecimento.

### O Context Index

O Context Index é a base de conhecimento estruturada da qual os agents se beneficiam durante a execução. Ele inclui:

- **Live Specs** — Especificações legíveis por máquina para itens de trabalho atuais
- **System Constitution** — Princípios arquitetônicos, padrões de codificação, políticas de segurança
- **Codebase context** — Definições de tipo, schemas de API, suítes de teste, documentação
- **Historical context** — Decisões passadas, bloqueadores resolvidos, lições aprendidas

O Context Index serve como base de conhecimento persistente do agent. Quanto mais rico e estruturado ele for, mais autonomamente o agent poderá operar. Para um tratamento completo do Context Index, padrões de retrieval e práticas de higiene, consulte [Context Management](/en/handbook/infrastructure/context-management).

Organizações que adotam padrões [[rag|RAG]] para apresentar context relevante dinamicamente podem reduzir ainda mais a carga sobre o Context Architect e aumentar a proporção de tarefas que os agents executam autonomamente.

## Pilar 2: Infraestrutura Efêmera

Cada execução de agent acontece dentro de um Ephemeral Workbench — um ambiente isolado, seguro e descartável que contém tudo o que o agent precisa e nada que não seja necessário.

### Workbenches como MicroVMs Isoladas

Um Ephemeral Workbench é um ambiente de execução em sandbox provisionado sob demanda e destruído após o uso. Cada workbench:

- Contém uma cópia limpa do codebase relevante e suas dependências
- Tem acesso apenas ao context e às ferramentas especificadas no Live Spec da tarefa
- Não pode modificar o estado compartilhado, sistemas de produção ou workbenches de outros agents
- Produz artefatos (mudanças de código, resultados de teste, logs) que são extraídos antes do teardown

Este modelo elimina uma classe inteira de riscos: os agents não podem corromper acidentalmente ambientes compartilhados, vazar secrets entre tarefas ou criar efeitos colaterais persistentes.

### Segurança por Design

A infraestrutura efêmera torna a segurança uma propriedade estrutural, em vez de uma sobreposição de política:

- **Blast radius containment** — Se um agent fizer algo errado, o dano é limitado a um ambiente descartável
- **No persistent access** — Os agents nunca possuem credenciais ou sessões de longa duração
- **Auditable by default** — Cada execução de workbench produz um log completo e reproduzível
- **Resistência a [[prompt-injection]]** — Ambientes isolados limitam o que um invasor pode alcançar, mesmo que comprometa a entrada de um agent

## Pilar 3: Governança Baseada em Gates

O desenvolvimento agentic substitui a revisão de código informal por um sistema estruturado de checkpoints automatizados e human-in-the-loop.

### Gates Automatizados Contínuos

Esses gates são executados em cada execução do agent sem envolvimento humano:

- **Security scans** — Análise estática, verificações de vulnerabilidade de dependência, detecção de secrets
- **Eval Harness** — Os critérios de aceitação definidos pela spec, executados como testes automatizados
- **Architectural conformance** — Verifica se as mudanças seguem padrões estabelecidos e não introduzem dependências proibidas
- **Performance baselines** — Benchmarks automatizados que detectam regressões

O Eval Harness é a peça central. Ele transforma os critérios de aceitação de um documento que os humanos leem em verificações executáveis que os agents devem passar. Este é o mecanismo que torna a execução autônoma confiável.

### Gates HITL Obrigatórios

Algumas decisões são muito importantes para automação completa. Os gates human-in-the-loop obrigatórios incluem:

- **Mudanças críticas de segurança** — Fluxos de autenticação, lógica de autorização, criptografia de dados
- **Decisões arquitetônicas** — Novos limites de serviço, mudanças de schema de banco de dados, contratos de API
- **Deployments de alto blast-radius** — Mudanças que afetam todos os usuários ou infraestrutura crítica
- **Padrões novos** — Implementações pioneiras de abordagens não cobertas por specs existentes

A principal percepção é que o risco de [[hallucination]] não é uniforme em todas as tarefas. A Governança Baseada em Gates aloca a atenção humana onde ela é mais importante, em vez de distribuí-la de forma diluída por tudo.

## Pilar 4: Engenharia Híbrida

Engenharia Híbrida é a prática de rotear cada tarefa para o recurso mais eficiente — seja um agent autônomo, um agent assistido ou um engenheiro humano.

### Auto-Agents para Tarefas Padrão

Tarefas bem definidas, com padrões existentes e de baixo risco são ideais para execução totalmente autônoma. Exemplos incluem:

- Implementar endpoints CRUD a partir de specs de API
- Escrever unit tests para funções existentes
- Aplicar padrões de refactoring em um codebase
- Gerar documentação a partir de código e definições de tipo

### Engenheiros Humanos para Trabalho Complexo

Tarefas que envolvem alta ambiguidade, arquitetura nova ou decisões significativas de julgamento permanecem com engenheiros humanos. Exemplos incluem:

- Projetar novas arquiteturas de sistema
- Resolver requisitos conflitantes
- Tomar decisões de trade-off de segurança
- Lidar com domínios de problemas novos sem padrões existentes

### A Decisão de Roteamento

O Sistema de Orquestração roteia as tarefas com base em uma combinação de fatores:

- **Spec completeness** — Quão bem definidos estão os critérios de aceitação?
- **Pattern coverage** — O Context Index contém precedentes relevantes?
- **Blast radius** — Qual é o impacto potencial de uma implementação errada?
- **Novelty** — Isso é uma variação de um padrão conhecido ou algo inteiramente novo?

Com o tempo, à medida que o Context Index cresce e os specs se tornam mais precisos, a proporção de tarefas de auto-agent aumenta. É assim que as equipes agentic escalam: não contratando mais engenheiros, mas expandindo o limite do que os agents podem lidar autonomamente.

## Próximos Passos

Esses quatro pilares fornecem a base estrutural. A próxima página aborda [Spec-Driven Development](/en/handbook/framework/spec-driven-development), a prática que substitui user stories informais por especificações legíveis por máquina — a entrada primária que torna esses pilares operacionais.
