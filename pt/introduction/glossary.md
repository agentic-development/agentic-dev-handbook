---
title: Glossário do Framework
description: Definições de referência rápida para termos-chave usados em todo o manual
authors:
  - dpavancini
lastUpdated: '2026-02-25'
---

Este glossário define os termos chave introduzidos no manual. Cada entrada inclui uma breve definição, um link para a entrada completa do glossário no site e uma referência à página do manual onde o conceito é explicado em detalhe.

## Termos

**[[agentops-dashboard|AgentOps Dashboard]]** — A interface de monitoramento em tempo real que rastreia o status de execução do agent, a saúde do pipeline e os custos de computação em um squad. O Flow Manager a revisa durante a Daily Flow Sync.
Handbook: [Cerimônias e Rotinas](/en/handbook/operations/ceremonies)

**[[blocker-flag|Blocker Flag]]** — Um sinal levantado por um agent quando ele encontra ambiguidade ou uma restrição que não consegue resolver, desencadeando uma escalada para um operador humano que executa uma Agent Recovery.
Handbook: [A Camada de Orquestração](/en/handbook/framework/orchestration-layer)

**[[context-engineering|Context Engineering]]** — A prática de projetar, organizar e gerenciar as informações que fluem para a context window de um LLM. Adota uma visão em nível de sistema de como os system prompts, documentos, histórico e ferramentas são montados e priorizados.
Handbook: [Gerenciamento de Contexto](/en/handbook/infrastructure/context-management)

**[[context-index|Context Index]]** — A base de conhecimento estruturada da qual os agents se beneficiam durante a execução. Contém Live Specs, regras arquiteturais, Golden Samples, glossários de domínio e contexto histórico, organizados para consumo por máquina.
Handbook: [Gerenciamento de Contexto](/en/handbook/infrastructure/context-management)

**[[context-packet|Context Packet]]** — Uma coleção empacotada de especificações, regras arquiteturais, Golden Samples e contexto de domínio montada para uma tarefa específica. Criada durante os Specification Engineering Blocks semanais e consumida durante a execução diária do agent.
Handbook: [Cerimônias e Rotinas](/en/handbook/operations/ceremonies)

**[[continuous-development-loop|Continuous Development Loop]]** — A evolução agentic do CI/CD que automatiza todo o ciclo de vida, da spec à produção. Estende o pipeline tradicional com injeção de spec, montagem automatizada de contexto, portas de Eval Harness e loops de feedback de conhecimento.
Handbook: [De Agile para Agentic](/en/handbook/framework/from-agile-to-agentic)

**[[human-owned-core|Human-Owned Core]]** — Código que é muito sensível arquiteturalmente ou crítico para a segurança para a execução de agents. Escrito exclusivamente por Agent Operators humanos, inclui caminhos de autenticação, algoritmos centrais e abstrações fundamentais.
Handbook: [O Squad Híbrido](/en/handbook/team-model/hybrid-squad)

**[[eval-harness|Evaluation Harness]]** — O pacote de testes automatizado que valida cada saída do agent antes que ela chegue a um revisor humano. Combina testes funcionais, varreduras de segurança, verificações de conformidade arquitetural e avaliações LLM-as-a-Judge.
Handbook: [Evaluation Harness](/en/handbook/infrastructure/evaluation-harness)

**[[golden-samples|Golden Samples]]** — Implementações de referência curadas que os agents usam como modelos para um novo código. Um golden sample bem escolhido ensina um agent mais do que páginas de instruções escritas, demonstrando padrões esperados através de exemplos concretos.
Handbook: [O Squad Híbrido](/en/handbook/team-model/hybrid-squad)

**[[human-in-the-loop|Human-in-the-Loop]]** — Um princípio de design onde o julgamento humano é exigido em pontos de decisão críticos antes que um agent prossiga com ações consequentes. Permite autonomia graduada através de modelos de permissão em camadas.
Handbook: [A Camada de Orquestração](/en/handbook/framework/orchestration-layer)

**[[live-spec|Live Spec]]** — Um contrato determinístico e legível por máquina que define o que construir, por que é importante e como verificar seu funcionamento. Contém um Contrato Comportamental, Constituição do Sistema e Mapa de Tarefas Acionável. Evolui juntamente com a base de código sob controle de versão.
Handbook: [Desenvolvimento Orientado por Spec](/en/handbook/framework/spec-driven-development)

**[[agent-recovery|Agent Recovery]]** — O fluxo de trabalho de intervenção onde um Agent Operator diagnostica um agent travado, injeta contexto ausente e retoma a execução. Segue um processo de três etapas: Diagnosticar, Injetar, Retomar.
Handbook: [Cerimônias e Rotinas](/en/handbook/operations/ceremonies)

**[[spec-driven-development|Spec-Driven Development]]** — A prática de substituir user stories informais por especificações legíveis por máquina que servem como contratos determinísticos entre humanos e agents. Torna os pilares centrais operacionais, fornecendo entradas precisas para contexto, governança e roteamento.
Handbook: [Desenvolvimento Orientado por Spec](/en/handbook/framework/spec-driven-development)

**[[token-budget|Token Budget]]** — O gasto máximo de computação autorizado por período de tempo, funcionando como uma restrição rígida que impede loops de agent descontrolados. Imposto na Orchestration Layer e rastreado no AgentOps Dashboard.
Handbook: [Cerimônias e Rotinas](/en/handbook/operations/ceremonies)
