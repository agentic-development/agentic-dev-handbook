---
title: Sistemas Multiagente
description: >-
  Como múltiplos agentes AI se coordenam através de padrões de orquestração e
  padrões de interoperabilidade como MCP
authors:
  - dpavancini
lastUpdated: '2026-02-23'
---

## Visão Geral

Um [[multi-agent-systems|sistema multi-agente]] (MAS) distribui a execução do fluxo de trabalho entre múltiplos agentes de AI especializados que coordenam para resolver problemas que nenhum agente individual poderia lidar eficientemente sozinho. Em vez de construir um único agente monolítico que faz tudo, as arquiteturas multi-agente atribuem funções distintas a agentes focados — um agente de planejamento, um agente de codificação, um agente de teste — e definem como eles se comunicam e entregam o trabalho.

## Por Que Múltiplos Agentes?

Um único agente com acesso a dezenas de ferramentas e um prompt massivo de sistema torna-se cada vez mais não confiável à medida que a complexidade cresce. Sua context window se enche, suas instruções entram em conflito e sua tomada de decisões se degrada.

Sistemas multi-agente abordam isso aplicando o mesmo princípio que torna as equipes humanas eficazes: especialização. Cada agent tem uma função específica, um conjunto focado de ferramentas e instruções claras para seu domínio.

Considere um fluxo de trabalho de desenvolvimento de software:

- **Planning Agent** — Analisa requisitos, divide o trabalho em tarefas, define critérios de aceitação
- **Software Engineering Agent** — Escreve código de implementação, realiza refactoring, lida com migrações
- **QA Agent** — Gera testes, executa suítes de teste, valida o comportamento contra os critérios de aceitação
- **Review Agent** — Verifica a qualidade do código, segurança, aderência às convenções

Cada agente se destaca em sua função porque carrega apenas o context e as ferramentas relevantes para essa função. O sistema como um todo lida com fluxos de trabalho complexos que sobrecarregariam qualquer agente individual.

## Padrões de Orquestração

[[multi-agent-orchestration|Orquestração]] define como os agentes coordenam — quem decide o que acontece em seguida, como a informação flui entre os agentes e como os conflitos são resolvidos. A escolha do padrão de orquestração tem um impacto direto na confiabilidade, flexibilidade e complexidade do sistema.

### Orquestração Centralizada

Um único agente condutor (às vezes chamado de mestre ou supervisor) gerencia todo o fluxo de trabalho. Ele recebe a tarefa inicial, decide quais agentes especialistas invocar, passa o context entre eles e monta o resultado final.

**Como funciona:**

1. O condutor recebe uma tarefa do usuário
2. Ele divide a tarefa em subtarefas
3. Ele despacha cada subtarefa para o agente especialista apropriado
4. Ele coleta os resultados e decide o próximo passo
5. Ele monta a saída final

**Pontos Fortes:**

- Simples de raciocinar — um agente tem a visão completa
- Fácil de implementar logging e observabilidade
- Responsabilidade clara pelas decisões

**Pontos Fracos:**

- O condutor é um único ponto de falha
- Pode se tornar um gargalo à medida que o número de agentes especialistas cresce
- A context window do condutor deve acomodar o estado completo do fluxo de trabalho

### Orquestração Descentralizada

Agentes comunicam-se diretamente entre si de forma peer-to-peer. Não há um coordenador central — os agentes passam o trabalho para o próximo agente na cadeia com base em sua própria avaliação do que precisa acontecer em seguida.

**Como funciona:**

1. Um agente completa sua tarefa
2. Ele avalia o resultado e decide qual agente deve lidar com o próximo passo
3. Ele passa o context diretamente para esse agente
4. O processo continua até que o fluxo de trabalho seja concluído

**Pontos Fortes:**

- Não há um único ponto de falha
- Escala naturalmente à medida que os agentes são adicionados
- Cada agente precisa apenas do context para sua tarefa imediata

**Pontos Fracos:**

- Mais difícil de monitorar o estado geral do fluxo de trabalho
- Potencial para loops ou deadlocks se os agentes discordarem
- A depuração requer rastreamento em múltiplas interações de agentes

### Orquestração Híbrida

Combina abordagens centralizadas e descentralizadas. Um condutor gerencia o fluxo de trabalho de alto nível, enquanto permite que agentes especialistas coordenem diretamente entre si para subtarefas.

Este é o padrão mais comum em sistemas de produção. O condutor lida com a decomposição da tarefa e a montagem final, enquanto os agentes especialistas colaboram nos detalhes de implementação sem rotear todas as mensagens através do condutor.

### Padrão Broker/Mediator

Um agente broker atua como um roteador de mensagens sem entender o fluxo de trabalho em si. Ele recebe solicitações, as combina com o agente mais apropriado com base nas capacidades e roteia a resposta de volta.

Este padrão é útil quando você tem um grande pool de agentes intercambiáveis e deseja balancear a carga ou rotear com base na disponibilidade. O broker não planeja ou raciocina sobre o fluxo de trabalho — ele simplesmente conecta os solicitantes aos provedores.

### Orquestração Baseada em Fluxo de Trabalho

A lógica de orquestração é definida externamente como um grafo de fluxo de trabalho (um DAG ou máquina de estados) em vez de ser incorporada em um agente. Os agentes são invocados em nós específicos do grafo, e as transições entre os nós são determinadas pelo motor do fluxo de trabalho.

**Pontos Fortes:**

- A lógica do fluxo de trabalho é explícita e auditável
- Fácil de modificar sem alterar o código do agente
- Bem adequado para ambientes regulamentados que exigem caminhos determinísticos

**Pontos Fracos:**

- Menos adaptável — os agentes não podem alterar dinamicamente o fluxo de trabalho
- Requer design de fluxo de trabalho prévio
- Pode não lidar com situações inesperadas de forma elegante

## Model Context Protocol (MCP)

À medida que os sistemas de agent crescem, eles precisam interagir com um número crescente de ferramentas externas, fontes de dados e serviços. O [[mcp|Model Context Protocol (MCP)]] é um padrão aberto introduzido pela Anthropic para resolver esse desafio de interoperabilidade.

### O Problema que o MCP Resolve

Sem um protocolo padrão, cada integração de ferramenta de AI é customizada. Se você tem 5 modelos de AI e 10 ferramentas externas, você precisa de 50 integrações customizadas. Adicionar um novo modelo significa construir mais 10. Isso não escala.

O MCP padroniza como os modelos de AI acessam ferramentas e dados externos, muito parecido com o USB que padronizou como os computadores se conectam a periféricos. Com o MCP, qualquer modelo compatível pode se conectar a qualquer ferramenta compatível através de um único protocolo.

### Como o MCP Funciona

A [[model-context-protocol|arquitetura MCP]] segue um modelo cliente-servidor:

- **MCP Client** — Integrado à aplicação de AI (por exemplo, Claude Code, Cursor). Ele descobre os servidores disponíveis, negocia capacidades e roteia [[tool-calling|chamadas de ferramentas]].
- **MCP Server** — Atua como uma ponte entre o modelo de AI e um sistema externo. Cada servidor expõe um conjunto definido de ferramentas e recursos. Por exemplo, um servidor MCP do GitHub pode expor ferramentas para criar pull requests, ler issues e pesquisar repositórios.
- **Camada de transporte** — O MCP suporta múltiplos mecanismos de transporte (stdio, HTTP com SSE) para que os servidores possam ser executados local ou remotamente.

Um fluxo de interação típico:

1. A aplicação de AI conecta-se a um ou mais servidores MCP na inicialização
2. O MCP client consulta cada servidor para suas ferramentas e recursos disponíveis
3. Quando o agent precisa usar uma ferramenta, o client envia uma solicitação estruturada para o servidor apropriado
4. O servidor executa a operação e retorna o resultado
5. O client passa o resultado de volta para o agent

### Por Que o MCP Importa

O MCP traz vários benefícios práticos para sistemas multi-agente:

- **Escreva uma vez, use em todo lugar** — Uma integração de ferramenta construída como um servidor MCP funciona com qualquer modelo de AI compatível com MCP
- **Ecossistema comunitário** — O padrão aberto permite um ecossistema crescente de servidores MCP compartilhados para serviços comuns
- **Limites de segurança** — Os servidores MCP podem impor sua própria autenticação, autorização e rate limiting
- **Local e remoto** — Os servidores podem ser executados na máquina do desenvolvedor para acesso de baixa latência ou remotamente para ferramentas de equipe compartilhadas

## Interoperabilidade e Princípios DRY

Sistemas multi-agente se beneficiam enormemente dos mesmos princípios de engenharia que tornam o software tradicional manutenível.

### Não Repita Você Mesmo (DRY)

Quando múltiplos agentes precisam da mesma capacidade — digamos, ler um banco de dados ou chamar uma API — essa capacidade deve existir em um único lugar. Na prática, isso significa:

- **Bibliotecas de ferramentas compartilhadas** — Construir ferramentas comuns uma vez e torná-las disponíveis para todos os agentes através de uma interface padrão (como o MCP)
- **Configuração centralizada** — Armazenar configurações compartilhadas (endpoints de API, credenciais, parâmetros de modelo) em um único local
- **Componentes de prompt reutilizáveis** — Extrair padrões de instrução comuns em templates compartilhados que múltiplos agentes incluem

### Padronizar a Comunicação

Agentes que se comunicam através de protocolos bem definidos são mais fáceis de desenvolver, testar e substituir. Quando o Agente A envia uma solicitação de code review ao Agente B, o formato da mensagem deve ser consistente e documentado. Isso permite que você troque o Agente B por uma implementação diferente sem alterar o Agente A.

### Projetar para Substituibilidade

Construa agentes de forma que qualquer agente individual possa ser substituído sem redesenhar o sistema. Isso significa:

- Interfaces claras entre os agentes
- Sem dependências ocultas ou estado mutável compartilhado
- Contratos de entrada/saída bem documentados

Quando um modelo melhor surgir ou um agente especialista precisar ser reescrito, a mudança deve ser localizada apenas naquele agente.

## O Que Vem a Seguir

Com a compreensão dos agentes individuais e como eles coordenam em sistemas multi-agente, o próximo capítulo muda o foco para os frameworks e metodologias práticas para colocar esses conceitos em prática em equipes de desenvolvimento reais.
