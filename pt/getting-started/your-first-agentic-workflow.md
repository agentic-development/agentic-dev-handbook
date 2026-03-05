---
title: Seu Primeiro Fluxo de Trabalho Agêntico
description: >-
  Um guia passo a passo para completar uma tarefa com um agente de codificação
  de AI
authors:
  - dpavancini
lastUpdated: '2026-02-19'
---

## Visão Geral

Esta página detalha um fluxo de trabalho completo de desenvolvimento agentic — desde a definição de uma tarefa até a revisão do resultado. Usaremos um exemplo simples: adicionar uma função de utilidade com testes.

## O Fluxo de Trabalho

O desenvolvimento agentic segue um ciclo previsível:

1.  **Definir** — Declare claramente o que você quer
2.  **Contexto** — Garanta que o agent tenha o que precisa
3.  **Executar** — Deixe o agent trabalhar
4.  **Revisar** — Verifique a saída
5.  **Iterar** — Refine se necessário

## Passo 1: Definir a Tarefa

Boas definições de tarefa são específicas e delimitadas. Compare:

**Vago:** "Deixe o aplicativo mais rápido"

**Específico:** "Adicione uma função de utilidade `debounce` em `src/utils/debounce.ts` que aceita uma função e um atraso em milissegundos, e retorna uma versão com debounce. Inclua testes de unidade."

Quanto mais precisa for sua solicitação, melhor será o resultado.

## Passo 2: Fornecer Contexto

Aponte o agent para os arquivos relevantes:

-   Funções de utilidade existentes (para consistência de estilo)
-   A configuração do framework de teste
-   Convenções de tipo usadas no projeto

A maioria das ferramentas detecta o context automaticamente, mas referências explícitas ajudam.

## Passo 3: Deixar o Agent Trabalhar

Inicie a tarefa e observe. Um agent bem configurado irá:

1.  Ler arquivos relacionados para entender as convenções
2.  Escrever a implementação
3.  Escrever testes
4.  Executar os testes para verificar
5.  Corrigir quaisquer problemas

Resista à tentação de interromper no meio do processo. Deixe o agent completar seu ciclo e, em seguida, revise.

## Passo 4: Revisar a Saída

Verifique o trabalho do agent como você revisaria um pull request de um colega:

-   **Corretude** — Faz o que você pediu?
-   **Estilo** — Corresponde às convenções do projeto?
-   **Casos de borda** — As condições de limite são tratadas?
-   **Testes** — São significativos, não apenas passando?
-   **Sem extras** — Adicionou código ou dependências desnecessárias?

## Passo 5: Iterar

Se algo precisar de ajuste, seja específico:

**Em vez de:** "Corrija isso"

**Tente:** "A função debounce deve cancelar chamadas pendentes quando o componente é desmontado. Adicione um método `cancel` à função retornada."

## Padrões Comuns

À medida que você ganha experiência, reconhecerá padrões que funcionam bem com agents. Explore a seção [Padrões](/en/patterns) para uma coleção selecionada, incluindo:

-   [Prompt Chaining](/en/patterns) — Quebrando tarefas complexas em etapas sequenciais
-   Test-Driven Development — Escrevendo testes primeiro e, em seguida, fazendo o agent implementar

## Dicas para o Sucesso

-   **Comece pequeno** — Construa confiança com tarefas simples antes das complexas
-   **Seja explícito** — Agents seguem instruções literalmente; a ambiguidade leva a surpresas
-   **Confie, mas verifique** — Sempre revise o código gerado antes de fazer o merge
-   **Aprenda as ferramentas** — Cada ferramenta de AI tem pontos fortes únicos; aprenda a aproveitá-los
-   **Itere rápido** — Ciclos de feedback rápidos produzem melhores resultados do que especificações longas

## Próximos Passos

Você está pronto para começar a usar o desenvolvimento agentic em seus projetos. Explore o restante deste manual para aprofundar-se em tópicos específicos, ou navegue pelos [Templates](/en/templates) para prompts prontos para uso.
