---
title: Do Agile ao Agentic
description: >-
  Como o framework de Desenvolvimento Agentic evolui as práticas Agile para
  equipes de software nativas de AI
authors:
  - dpavancini
lastUpdated: '2026-02-23'
---

## Visão Geral

O Agile moveu o desenvolvimento de software de processos rígidos e orientados por planos para uma entrega iterativa e centrada nas pessoas. O [[agentic-engineering|Desenvolvimento Agêntico]] é a próxima evolução, preservando a ênfase do Agile em feedback e adaptabilidade, ao mesmo tempo que muda fundamentalmente o que restringe a entrega, quem (ou o quê) realiza o trabalho e como a qualidade é assegurada.

## Por Que o Agile Sozinho Não é Suficiente

O Agile foi projetado para um mundo onde desenvolvedores humanos são o gargalo de execução. Suas cerimônias, papéis e artefatos otimizam a coordenação do esforço humano em iterações de tempo limitado. Esse modelo se rompe quando **agents** autônomos podem executar tarefas bem definidas em minutos, em vez de dias.

As tensões centrais:

-   **Sprints assumem ritmo humano.** Iterações de duas semanas fazem sentido quando a implementação leva dias. Quando **agents** podem implementar uma funcionalidade em minutos, os limites da **Sprint** tornam-se restrições artificiais em vez de horizontes de planejamento úteis.
-   **User stories assumem interpretação humana.** "Como usuário, quero..." depende do julgamento de um desenvolvedor para preencher lacunas. **Agents** precisam de precisão legível por máquina, não flexibilidade narrativa.
-   **Code review manual não escala.** Quando **agents** produzem código em volume, revisores humanos tornam-se o gargalo. A avaliação automatizada deve lidar com a maior parte da garantia de qualidade.

O desenvolvimento agêntico não abandona o Agile — ele o evolui. Os valores de colaboração com o cliente, resposta à mudança e software funcionando em detrimento de documentação permanecem. As práticas mudam para se adequar a um novo modelo de execução.

## Principais Diferenças

### De Restrição de Tempo para Restrição de Context

No Agile, a restrição principal é o tempo. As equipes planejam o que podem entregar dentro de uma **Sprint**, e a velocidade mede **story points** por iteração.

No Desenvolvimento Agêntico, a restrição principal é o **context**. A velocidade de entrega é determinada pela clareza com que a intenção é especificada e pela cobertura completa do domínio do problema pelo Context Index. Quando o **context** é excelente, os **agents** entregam na velocidade da máquina. Quando o **context** é ruim, os **agents** param, alucinam ou produzem trabalho que falha na validação.

Essa mudança altera o que as equipes otimizam. Em vez de remover bloqueadores dos calendários dos desenvolvedores, os líderes se concentram em melhorar a qualidade da especificação, enriquecer o Context Index e reduzir a ambiguidade nas definições de tarefas.

### Governança Contínua e Controle de Qualidade

O Agile geralmente depende de *quality gates* pós-implementação: **code review**, testes de QA, ambientes de *staging*. Estes ocorrem depois que um desenvolvedor escreveu o código.

O desenvolvimento agêntico move o controle de qualidade para antes e durante a execução:

-   **Evals** automatizadas pré-código verificam se a especificação em si está bem formada, completa e livre de contradições antes que qualquer **agent** toque na base de código.
-   A avaliação em tempo real é executada continuamente durante a execução do **agent**, capturando problemas à medida que surgem, em vez de depois do fato.
-   A progressão de *gate* automatizada substitui a revisão manual para tarefas padrão, com revisão humana reservada para mudanças de alto risco.

O resultado é um modelo de qualidade que escala com o *throughput* do **agent**, em vez de ser gargalo pela disponibilidade de revisores humanos.

### Novo Papel do Humano

O Agile posiciona os desenvolvedores como artesãos que escrevem código, participam de cerimônias e tomam decisões de implementação. O desenvolvimento agêntico muda o papel humano de executor para diretor:

-   **Context Architects** definem o que precisa acontecer e porquê, em vez de implementar eles próprios.
-   **Agent Operators** supervisionam a execução e intervêm em exceções, em vez de lidar com cada tarefa pessoalmente.
-   **Technical Reviewers** validam o ajuste arquitetural e a segurança, em vez de revisar cada linha de código.

Isso não significa que menos habilidade seja necessária. Dirigir **agents** eficazmente exige um profundo entendimento técnico — é preciso saber como um bom resultado se parece para especificá-lo precisamente e validá-lo eficientemente.

### Infraestrutura Efêmera

Equipes Agile tipicamente compartilham ambientes de desenvolvimento, servidores de *staging* e *pipelines* de **CI**. Equipes agênticas provisionam *workbenches* isolados e descartáveis para cada tarefa. Isso transforma a infraestrutura de um recurso compartilhado e persistente para um sob demanda e efêmero.

A implicação: a infraestrutura se torna um custo por tarefa, em vez de um custo fixo da equipe, e a configuração do ambiente torna-se parte da especificação, em vez de uma preocupação separada de **DevOps**.

## A Tabela de Comparação

| Dimensão                  | Waterfall                 | Agile                             | Agentic                                    |
| :------------------------ | :------------------------ | :-------------------------------- | :----------------------------------------- |
| **Restrição Primária**    | Escopo (requisitos fixos) | Tempo (limites de **Sprint**)     | **Context** (clareza da especificação)   |
| **Unidade de Trabalho**   | Documento de requisito    | **User story**                    | Especificação Viva                         |
| **Ciclo de Execução**     | Fases sequenciais        | **Sprints** de tempo limitado (1-4 semanas) | Loop contínuo de especificação para **deploy** |
| **Artefato Primário**     | Documento de especificação | Incremento de software funcionando | Código validado + Context Index enriquecido |
| **Controle de Qualidade** | Revisões de *phase-gate*   | **Sprint review** + retrospectiva | **Eval Harness** automatizado + *gates* HITL |
| **Papel do Humano**       | Autor e executor          | Artesão e colaborador             | Diretor e validador                        |
| **Infraestrutura**        | Ambientes compartilhados, de longa duração | **CI/CD pipelines** compartilhados | *Workbenches* efêmeros, por tarefa          |

## Do CI/CD ao Continuous Development Loop

O Continuous Development Loop (CDL) estende o **CI/CD** para cobrir todo o ciclo de vida agêntico. Onde o **CI/CD** automatiza do **commit** de código ao **deploy**, o CDL automatiza da criação da especificação ao **deploy** e vice-versa.

### Mapeamento de Fases

| Fase CDL              | Equivalente CI/CD                   | Melhoria Chave                                              |
| :-------------------- | :---------------------------------- | :---------------------------------------------------------- |
| Injeção de Especificação | Criação de **branch** / início de *ticket* | Especificação estruturada, legível por máquina, substitui *ticket* informal |
| Montagem de **Context**   | Integração do desenvolvedor à tarefa  | Recuperação automatizada do Context Index                  |
| Execução Autônoma     | Escrita de código                   | Implementação orientada por **agent** em *workbench* isolado |
| **Eval Harness**      | Conjunto de testes **CI**             | Verificações comportamentais, de segurança e arquitetônicas além dos testes de unidade |
| Revisão de *Gate*     | **Pull request review**             | *Gates* automatizados para tarefas padrão; HITL para alto risco |
| Atualização de **Context** | Atualização de documentação         | Enriquecimento automático do Context Index a partir dos resultados da execução |
| **Deployment**        | **CD pipeline**                     | Infraestrutura de **deployment** padrão, inalterada        |

### Principais Melhorias Arquitetônicas

Três capacidades distinguem o CDL do **CI/CD** tradicional:

1.  **Segurança como um fio contínuo.** Em vez de uma etapa de revisão de segurança separada, as verificações de segurança são executadas em todas as fases — da validação da especificação à execução e ao **deploy**. Isso captura problemas de segurança no ponto de introdução, em vez de depois do fato.
2.  O **Eval Harness** substitui o **CI** como o *quality gate* primário. O **CI** tradicional executa testes de unidade e *linters*. O **Eval Harness** executa verificação comportamental, verificações de conformidade arquitetônica, *baselines* de desempenho e varreduras de segurança como um conjunto de avaliação unificado.
3.  **Feedback loops centrados no conhecimento.** Cada ciclo CDL produz dados estruturados que alimentam o Context Index. Avaliações falhas tornam-se padrões documentados. Intervenções de operadores tornam-se atualizações de **context**. Execuções bem-sucedidas reforçam abordagens comprovadas. O sistema aprende continuamente.

## O Surgimento dos Coding Agents

A transição do Agile para Agêntico é possibilitada por uma rápida evolução nas ferramentas de codificação **AI**, passando de assistentes passivos para **agents** autônomos.

### De Copilots para Autonomous Teams

A trajetória das ferramentas de codificação **AI** segue uma progressão clara:

1.  **Code completion** (2021-2023) — Ferramentas de sugestão de código *inline*. GitHub Copilot, TabNine e outros sugerem a próxima linha ou bloco de código. O humano permanece em controle total.
2.  **Interactive assistants** (2023-2024) — Interfaces baseadas em *chat* como ChatGPT, **Claude**, e o modo *composer* do Cursor. Desenvolvedores descrevem o que querem em linguagem natural e iteram na saída. Ainda orientado por humanos, mas com maior alavancagem.
3.  **Coding agents** (2024-2025) — Ferramentas como **Claude** Code, modo **agent** do GitHub Copilot e **agents** de *background* do Cursor que podem planejar implementações multi-passos, executá-las em vários arquivos, rodar testes e iterar em falhas. O humano define a tarefa; o **agent** lida com a execução.
4.  **Autonomous teams** (2025-presente) — Sistemas **multi-agent** onde **agents** especializados lidam com diferentes aspectos do desenvolvimento (planejamento, implementação, teste, revisão) com supervisão humana em pontos de controle estratégicos. É aqui que o [[vibe-coding]] evolui de uma atividade solo para um *workflow* orquestrado.

### Capacidades dos Generalist Agents

**Coding agents** modernos são generalistas. Um único **agent** pode:

-   Ler e entender bases de código inteiras
-   Planejar implementações multi-passos antes de escrever código
-   Escrever código em vários arquivos e linguagens
-   Executar e interpretar conjuntos de testes
-   Depurar falhas e iterar em soluções
-   Interagir com ferramentas de desenvolvimento (**Git**, gerenciadores de pacotes, sistemas de *build*, **APIs**)
-   Seguir convenções específicas do projeto definidas em arquivos de **context**

Essa capacidade generalista é o que torna o modelo de [[software-factory]] viável. Você não precisa de uma ferramenta diferente para cada tarefa — um único **agent** bem contextualizado pode lidar com toda a gama de trabalho de desenvolvimento dentro de seu limite de competência.

### Comparação de Plataformas

O cenário dos **coding agents** está evoluindo rapidamente. No início de 2026, as principais plataformas incluem:

| Plataforma                     | Abordagem                           | Pontos Fortes                                         |
| :----------------------------- | :---------------------------------- | :---------------------------------------------------- |
| **Claude Code** (Anthropic)    | **Agent** nativo de **CLI** com uso profundo de ferramentas | Alterações em vários arquivos, refatoração complexa, integração com terminal |
| **GitHub Copilot / Codex** (Microsoft) | **Agent** integrado ao **IDE** + **cloud agent** | Integração com o ecossistema **GitHub**, execução assíncrona em *background* |
| **Gemini Code Assist / Jules** (Google) | **IDE** plugin + **autonomous agent**        | Suporte a **multi-model**, integração com Google Cloud |
| **Open Source** (Aider, OpenCode, SWE-agent) | **Agents CLI** impulsionados pela comunidade | Transparência, customização, flexibilidade de modelo |

A escolha da plataforma importa menos do que as práticas em torno dela. Uma equipe com especificações excelentes, um Context Index rico e governança [[human-in-the-loop]] disciplinada superará uma equipe com um **agent** "melhor", mas com **context** deficiente e *workflows* ad-hoc.

## Fazendo a Transição

Equipes que migram do Agile para Agêntico não precisam mudar tudo de uma vez. Um caminho prático:

1.  **Comece com as especificações.** Escolha uma equipe ou um projeto e exija especificações legíveis por máquina em vez de (**user stories**) (ou em adição a elas). Meça se a qualidade da saída do **agent** melhora.
2.  **Introduza o Eval Harness.** Adicione verificações comportamentais automatizadas junto ao **CI** existente. Execute-as tanto em código humano quanto em código de **agent** para estabelecer *baselines*.
3.  **Pilote a execução autônoma.** Encaminhe tarefas bem definidas e de baixo risco para **agents**. Mantenha a revisão humana no *loop* inicialmente para construir confiança.
4.  **Construa o Context Index.** Capture as lições aprendidas com as execuções de **agents**, intervenções de operadores e feedback de revisão em um formato estruturado e pesquisável.
5.  **Expanda o limite.** À medida que a confiança aumenta, encaminhe mais tipos de tarefas para **agents** e direcione o esforço humano para a qualidade da especificação, arquitetura e decisões estratégicas.

O objetivo não é substituir as cerimônias Agile da noite para o dia. É evoluí-las incrementalmente à medida que os **agents** assumem mais da carga de trabalho de execução, liberando os humanos para se concentrarem no trabalho que só humanos podem fazer.

## Próximos Passos

Com a estrutura definida, o próximo capítulo aborda o [Team Model](/en/handbook/team-model) —como estruturar papéis, habilidades e responsabilidades em uma organização de desenvolvimento agêntico.
