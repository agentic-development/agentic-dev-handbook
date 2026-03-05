---
title: Princípios de Desenvolvimento Agêntico
description: Dez princípios que definem o desenvolvimento agêntico como uma disciplina
authors:
  - dpavancini
lastUpdated: '2026-02-24'
---

## Visão Geral

Há uma confusão generalizada no mercado entre "entregar código" e "desenvolver software". LLMs podem gerar código sintaticamente correto em escala — esse problema está resolvido. Mas desenvolver software — entender requisitos, gerenciar a complexidade, garantir a confiabilidade, governar as mudanças — continua sendo um desafio profundamente humano.

[[agentic-engineering|Desenvolvimento agentic]] aborda esse desafio através da colaboração estruturada entre humanos e agents. Os dez princípios a seguir definem o que entendemos por desenvolvimento agentic e guiam cada capítulo deste manual.

## Em Resumo

1.  **Agents constroem o código; humanos direcionam o sistema.**
2.  **Context é o gargalo, não a velocidade de digitação.**
3.  **O código se torna cada vez mais efêmero; as specs perduram.**
4.  **As melhores práticas de software engineering se expandem, não desaparecem.**
5.  **As specs devem ser estruturadas e legíveis por máquina.**
6.  **Todo o SDLC é redesenhado para a colaboração humano-agent.**
7.  **Novos caminhos de expertise substituem os antigos.**
8.  **As equipes incluem explicitamente tanto agents quanto humanos.**
9.  **A governança empresarial é integrada, não acoplada.**
10. **Vibe coding não é desenvolvimento agentic.**

## O Que Acreditamos

### 1. Agents Constroem o Código; Humanos Direcionam o Sistema

O desenvolvedor humano passa de escrever código para direcionar, revisar e orquestrar os agents que o produzem.

### 2. Context É o Gargalo

Na era agentic, a restrição não é mais a velocidade de digitação — é o [[context-engineering|context]]. Construir um backlog bem estruturado que os agents possam consumir é o maior desafio que as equipes enfrentarão. O capítulo Framework explora isso através da Arquitetura Context-First.

### 3. O Código se Torna Cada Vez Mais Efêmero

À medida que as capacidades dos agents amadurecem, o código se torna cada vez mais efêmero — recompilável a partir de especificações com intervenção humana mínima. A spec, não o código, torna-se o artefato durável. O capítulo Infraestrutura aborda isso através do padrão Ephemeral Workbench.

### 4. As Melhores Práticas se Expandem, Não Desaparecem

O desenvolvimento agentic não substitui as melhores práticas de software engineering — ele as expande. Testes, code review e arquitetura importam mais, não menos, quando agents estão produzindo código em escala. O capítulo Operações detalha como as práticas tradicionais evoluem através de cerimônias agentic.

## O Que Fazemos

### 5. As Specs Devem Ser Estruturadas e Legíveis por Máquina

O desenvolvimento agentic espera especificações bem projetadas e legíveis por máquina. Agents precisam de inputs estruturados e inequívocos para produzir outputs confiáveis. O pilar de Desenvolvimento Orientado a Spec do capítulo Framework aborda isso em profundidade.

### 6. Todo o SDLC é Redesenhado

O desenvolvimento agentic repensa todo o Software Development Lifecycle — do escopo do projeto e planejamento do trabalho à execução e deployment. Cada fase é redesenhada para a colaboração humano-agent. Veja a comparação de abordagens Waterfall, Agile e Agentic do capítulo Framework.

### 7. Novos Caminhos de Expertise Substituem os Antigos

A era agentic afetará profundamente como os engenheiros constroem expertise. Novas habilidades surgem — [[context-engineering]], escrita de especificações, orquestração de agents — enquanto a fluência em codificação tradicional se torna pré-requisito. O capítulo Modelo de Equipe define seis novas funções construídas em torno dessas habilidades.

## Como Estruturamos

### 8. As Equipes Incluem Tanto Agents Quanto Humanos

O software será desenvolvido por equipes que explicitamente incluem tanto agents quanto humanos. Isso não é uma metáfora — é um princípio de design que afeta tooling, workflows e a estrutura da equipe. O capítulo Modelo de Equipe apresenta o Hybrid Squad como a unidade fundamental.

### 9. A Governança Empresarial é Integrada

O desenvolvimento de software empresarial exige responsabilidade e governança por design, mesmo quando agents produzem o código. A aprovação humana — explícita ou implícita — é exigida em cada ponto crítico. O pilar de Governança Baseada em Gates do capítulo Framework e as cerimônias do capítulo Operações tornam isso concreto.

### 10. Vibe Coding Não é Desenvolvimento Agentic

[[vibe-coding]] é primariamente um método individual de codificação — rápido, exploratório e não estruturado. O desenvolvimento agentic é uma disciplina de equipe com governança, repetibilidade e responsabilidade integradas.

## O Que Vem a Seguir

Estes princípios são a fundação — o restante do manual os torna concretos. Continue para [Para Quem É Este Manual?](/en/handbook/introduction/who-is-this-for) para ver como este manual serve a diferentes funções, ou pule diretamente para o capítulo [Framework](/en/handbook/framework) para explorar os cinco pilares que colocam esses princípios em prática.
