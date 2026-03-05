---
title: Desenvolvimento Orientado a Especificações
description: >-
  Como especificações legíveis por máquina substituem histórias de usuário como
  a unidade de trabalho no desenvolvimento agêntico
authors:
  - dpavancini
lastUpdated: '2026-02-24'
---

## Visão Geral

O Desenvolvimento Orientado a Especificações (Spec-Driven Development) substitui histórias de usuários informais e descrições de tickets por especificações legíveis por máquina que servem como contratos determinísticos entre humanos e agents. É a prática que torna os quatro Pilares Fundamentais operacionais — sem specs precisas, o context fica incompleto, os portões de governança não têm nada para verificar, e as decisões de roteamento carecem das informações necessárias.

## O que é uma Spec?

Uma spec é um contrato determinístico, legível por máquina, que define:

- **O que** precisa ser construído (requisitos comportamentais)
- **Por que** é importante (contexto de negócio e restrições)
- **Como verificar** se funciona (critérios de aceitação executáveis)

Ao contrário de uma user story ("Como usuário, eu quero..."), uma spec não deixa margem para interpretação. É precisa o suficiente para que um agent possa implementá-la sem fazer perguntas de esclarecimento e verificar seu próprio trabalho em relação aos critérios de aceitação.

## Os Três Assets Mínimos

Cada Live Spec deve conter pelo menos três assets:

1.  **Contrato Comportamental** — Uma descrição precisa do comportamento esperado, incluindo inputs, outputs, edge cases e tratamento de erros. Isso é o que o agent implementa.

2.  **Constituição do Sistema** — O [[system-prompt]] e as restrições que governam como o agent opera. Isso inclui padrões de codificação, padrões arquiteturais, políticas de segurança e quaisquer regras específicas do domínio. Ele define os limites das soluções aceitáveis.

3.  **Mapa de Tarefas Acionável** — Uma lista decomposta e ordenada de etapas de implementação que o agent pode seguir. Cada etapa faz referência à seção relevante do Contrato Comportamental e inclui seus próprios critérios de aceitação.

## Semelhanças com TDD

O Desenvolvimento Orientado a Especificações (Spec-Driven Development) compartilha o DNA com o Test-Driven Development (TDD). Ambas as abordagens definem o comportamento esperado antes de escrever a implementação. As principais diferenças:

-   **Escopo** — As specs do TDD são de nível de função; Live Specs cobrem features ou workflows inteiros
-   **Público** — Testes TDD são escritos para executores de teste; Live Specs são escritas para agents e humanos
-   **Riqueza** — Live Specs incluem context arquitetural, justificativa e decomposição que vão além do que um arquivo de teste captura

Equipes que já praticam TDD acharão a transição para o Spec-Driven Development natural. A disciplina de "definir o comportamento esperado primeiro" é a mesma — o formato e o escopo se expandem.

## Frameworks Alternativos

Vários frameworks surgiram para estruturar workflows orientados a specs:

-   **Método BMAD** — Um framework abrangente para definir tarefas de agent com prompts estruturados e especificações comportamentais
-   **GitHub Spec Kit** — A abordagem do GitHub para estruturar especificações para workflows orientados por Copilot
-   **Tessl** — Uma abordagem em nível de plataforma para orquestração de agent orientada a specs

Cada um adota um ângulo diferente, mas todos compartilham o princípio central: agents precisam de input estruturado e legível por máquina para produzir output confiável.

## Desafios

Adotar workflows orientados a specs não é isento de atritos:

-   **Percepção de velocidade perdida** — Escrever specs detalhadas parece mais lento do que "apenas codificar". As equipes devem internalizar que a spec é o trabalho, não uma sobrecarga antes do trabalho.
-   **Armadilha de manutenção** — Specs que não são mantidas em sincronia com a codebase tornam-se enganosas. Specs desatualizadas são piores do que nenhuma spec porque os agents as seguem fielmente.
-   **Lacuna de tradução** — Stakeholders de negócios pensam em resultados; agents precisam de contratos técnicos precisos. Preencher essa lacuna é a habilidade central do Arquiteto de Contexto.
-   **Degradação do context** — À medida que os sistemas crescem, o context total necessário para especificar uma mudança pode exceder o que cabe em uma única context window. O design modular de specs e a recuperação de context baseada em RAG mitigam isso.
-   **Ambiguidade semântica** — A linguagem natural é inerentemente ambígua. Mesmo specs bem escritas podem ser interpretadas de forma diferente por diferentes agents ou versões de modelo. Critérios de aceitação executáveis são o antídoto.

## O Conceito de Live Spec

Uma Live Spec não é um documento estático. É um blueprint modular, versionado, que evolui junto com a codebase:

-   **Versionada** — As specs vivem no repositório junto com o código que descrevem, com histórico completo do Git
-   **Modular** — Grandes features são decompostas em múltiplas specs que se referenciam
-   **Executável** — Os critérios de aceitação são escritos como verificações automatizadas, não como descrições em prosa
-   **Em Evolução** — As specs são atualizadas conforme os requisitos mudam, a implementação revela novos edge cases, ou a arquitetura do sistema se altera

Esta abordagem é uma evolução do [[prompt-driven-development]], movendo de prompts ad-hoc para especificações estruturadas e persistentes que acumulam conhecimento institucional.

## O Ciclo de Desenvolvimento Contínuo

Os quatro Pilares Fundamentais e o Spec-Driven Development se unem no Continuous Development Loop (CDL) — a evolução agêntica do CI/CD que automatiza todo o ciclo de vida, da spec à produção. O CDL estende o pipeline tradicional de build-test-deploy com injeção de specs, montagem automatizada de context, o Eval Harness como principal portão de qualidade, e loops de feedback centrados no conhecimento que enriquecem o Context Index após cada ciclo de execução.

Para uma análise detalhada das fases do CDL, seus equivalentes em CI/CD e aprimoramentos arquitetônicos chave, consulte [Da Agile à Agêntica](/en/handbook/framework/from-agile-to-agentic).

## Próximos Passos

Com os pilares arquitetônicos e as práticas orientadas a specs definidas, a próxima página explora como esses conceitos se mapeiam para cerimônias e papéis Agile familiares. [Da Agile à Agêntica](/en/handbook/framework/from-agile-to-agentic) aborda a tabela de comparação, a estratégia de transição e a evolução dos coding agents que torna essa mudança possível.
