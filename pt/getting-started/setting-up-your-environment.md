---
title: Configurando Seu Ambiente
description: >-
  Como configurar seu ambiente de desenvolvimento para fluxos de trabalho
  agenticos
authors:
  - dpavancini
lastUpdated: '2026-02-19'
---

## Visão Geral

Antes de trabalhar com AI coding agents, você precisa de um ambiente de desenvolvimento configurado corretamente. Esta página orienta você pelos elementos essenciais.

## Escolha Sua Ferramenta de AI Coding

Diversas ferramentas suportam fluxos de trabalho de desenvolvimento com agents:

-   **Claude Code** — O CLI agent da Anthropic para desenvolvimento baseado em terminal
-   **Cursor** — Editor de código AI-native construído sobre o VS Code
-   **GitHub Copilot** — AI pair programmer integrado ao VS Code e JetBrains
-   **Windsurf** — IDE da Codeium com tecnologia AI

Cada ferramenta tem pontos fortes diferentes. Claude Code se destaca em alterações multi-arquivo e refactoring complexo. Cursor oferece integração IDE robusta. Escolha a que melhor se adapta ao seu fluxo de trabalho — ou use várias para diferentes tarefas.

## Etapas Essenciais de Configuração

### 1. Controle de Versão

Fluxos de trabalho com agents fazem alterações frequentes. Uma configuração sólida de Git é inegociável:

-   Inicialize ou clone seu repository
-   Crie uma feature branch para trabalho com agents
-   Configure `.gitignore` corretamente
-   Considere usar git worktrees para sessões de agent paralelas

### 2. Contexto do Projeto

AI agents funcionam melhor com contexto. Forneça-o através de:

-   **README.md** — Visão geral do projeto, instruções de configuração, arquitetura
-   **CLAUDE.md / .cursorrules** — Instruções e convenções específicas do agent
-   **Type definitions** — TypeScript types, esquemas de API, modelos de banco de dados
-   **Test suites** — Testes existentes informam aos agents sobre o comportamento esperado

### 3. Configuração do Ambiente

```bash
# Exemplo: Configurando Claude Code
npm install -g @anthropic-ai/claude-code
cd your-project
claude
```

### 4. Rede de Segurança

Sempre tenha salvaguardas em vigor:

-   **Pre-commit hooks** — Lint e formate antes do commit
-   **CI/CD pipeline** — Testes automatizados pegam regressões
-   **Code review** — Revisão humana do código gerado pelo agent
-   **Branch protection** — Evite pushes diretos para a main

## Checklist de Verificação

Antes de iniciar o trabalho com agents, verifique:

-   [ ] repository Git inicializado com working tree limpo
-   [ ] Ferramenta de AI instalada e autenticada
-   [ ] Arquivos de contexto do projeto em seus devidos lugares (README, type definitions)
-   [ ] Linting e formatação configurados
-   [ ] Test suite executado com sucesso
-   [ ] CI/CD pipeline ativo

## Próximos Passos

Com seu ambiente pronto, siga para [Seu Primeiro Fluxo de Trabalho com Agents](/en/handbook/getting-started/your-first-agentic-workflow) para colocar tudo em prática.
