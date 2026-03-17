---
title: 'Estudo de Caso: Da Especificação à Produção'
description: >-
  Um exemplo de ponta a ponta de rotinas de desenvolvimento agentic aplicadas a
  um sistema de recomendação de e-commerce
authors:
  - dpavancini
lastUpdated: '2026-02-24'
---

## Visão Geral

Esta página detalha um exemplo completo de como as rotinas de governança definidas na página anterior funcionam juntas na prática. O cenário: uma equipe construindo um recurso de Recommender System para uma plataforma de e-commerce.

## Definição de Epic (Trimestral)

Durante a Definição Estratégica de Especificação, o Context Architect decompõe a iniciativa "Recomendações Personalizadas" em três Epics:

1. **User Behavior Tracking** — Capturar eventos de navegação, busca e compra.
2. **Recommendation Engine** — Construir o pipeline de ML que produz as recomendações.
3. **Frontend Integration** — Exibir as recomendações na UI do produto.

Cada Epic recebe uma Live Spec de alto nível. O Epic Recommendation Engine é classificado como de alto risco (arquitetura inovadora, complexidade do pipeline de ML) e configurado com portões HITL obrigatórios em todas as etapas.

## Live Spec Task (Semanal)

Durante o Bloco de Engenharia de Especificação, o Context Architect produz um Context Packet para a primeira tarefa no Epic User Behavior Tracking: "Implementar a captura de eventos para visualizações de página de produto."

O Context Packet inclui:

- **Live Spec** — Critérios de aceitação: capturar eventos de visualização de página com ID do produto, sessão do usuário, timestamp e dados de viewport. Casos de borda: usuários anônimos, filtragem de tráfego de bot, sessões concorrentes.
- **Architectural Rules** — Os eventos devem fluir através do barramento de eventos existente. Nenhuma gravação direta no banco de dados a partir da camada de captura. Os eventos devem estar em conformidade com o esquema CloudEvents da plataforma.
- **Golden Sample** — Uma implementação de referência da captura de eventos "adicionar ao carrinho" existente, mostrando os padrões corretos, convenções de nomenclatura e estrutura de teste.

## Agent Execution (Diário)

O Agent Operator alimenta o Context Packet a um Feature Agent. O prompt do Claude Code segue esta estrutura:

```
Você está implementando a captura de eventos para visualizações de página de produto.

Context Packet:
- Live Spec: [anexado]
- Architectural Rules: Eventos fluem através do EventBus. Sem gravações diretas no DB.
  Esquema CloudEvents necessário.
- Golden Sample: Veja a implementação do evento add-to-cart em
  src/events/add-to-cart.ts

Requisitos:
1. Implementar ProductPageViewEvent seguindo o esquema CloudEvents
2. Registrar o manipulador de eventos com o EventBus
3. Incluir filtragem de tráfego de bot usando o serviço BotDetector existente
4. Escrever testes de unidade cobrindo todos os critérios de aceitação e casos de borda

Restrições:
- Não modificar nenhum arquivo fora de src/events/ e src/events/__tests__/
- Seguir as convenções de nomenclatura mostradas no Golden Sample
- Todos os testes devem passar antes de enviar o PR
```

## Ciclo de Feedback Iterativo

O agent produz uma implementação inicial. O Evaluation Harness executa testes automatizados. Dois testes falham: a lógica de filtragem de bot não lida com o caso de borda em que uma string de user agent está ausente, e a validação do esquema CloudEvents rejeita o evento porque o campo `source` usa o formato errado.

O agent itera: corrige o tratamento de user-agent nulo, corrige o formato do campo `source` e reenviou. Todos os testes passam.

## Agent Recovery

Na segunda tarefa — "Implementar a captura de eventos de consulta de busca" — o agent trava. Ele tenta importar um módulo do domínio do recommendation engine, violando o limite de contexto delimitado. O Evaluation Harness sinaliza uma violação arquitetural, mas o agent não consegue resolvê-la porque não possui contexto sobre o padrão correto de comunicação interdomínio.

O Agent Operator intervém:

1. **Diagnostica** o problema: o contexto do agent não inclui a definição da interface AsyncMessageBus e um exemplo de publicação de evento entre domínios.
2. **Injeta** o contexto ausente: a definição da interface AsyncMessageBus e um exemplo de publicação de eventos entre domínios.
3. **Retoma** o agent, que agora publica corretamente o evento de busca através do message bus em vez de importar do domínio de recomendação.

## Conclusão da Tarefa

O Agent Operator revisa o pull request final. O código segue os padrões do Golden Sample, passa em todos os testes automatizados e respeita os limites arquiteturais. O pull request é aprovado e mergeado. O Flow Manager atualiza o AgentOps Dashboard, marcando a tarefa como concluída e registrando o consumo de token em relação ao orçamento semanal.

Tempo total decorrido do Context Packet para o pull request mergeado: 47 minutos para duas tarefas, incluindo uma Agent Recovery. O trabalho equivalente teria levado um desenvolvedor humano aproximadamente 1,5 a 2 dias.

## Principais Aprendizados

Este exemplo ilustra vários princípios da estrutura:

- **A qualidade do contexto determina a qualidade da execução.** A primeira tarefa foi bem-sucedida na segunda iteração porque o Context Packet era completo. A segunda tarefa exigiu uma Agent Recovery porque o padrão de comunicação interdomínio estava ausente do contexto.
- **O Evaluation Harness detecta erros que o agent não consegue autodiagnosticar.** Violações arquiteturais são invisíveis para o agent sem restrições explícitas. Verificações automatizadas detectaram o que o agent não conseguiu.
- **As Agent Recoveries são diagnósticas, não punitivas.** O operador não reescreveu o código — ele identificou o contexto ausente, injetou-o e deixou o agent concluir o trabalho.
- **O ciclo de vida completo se conecta.** O planejamento trimestral definiu os epics, a engenharia de especificação semanal produziu os context packets e a execução diária os consumiu. Cada cadência alimenta a próxima.

## O Que Vem a Seguir

A próxima página aborda as métricas e estruturas de acompanhamento de sucesso que medem se essas rotinas estão realmente funcionando — desde índices de alavancagem e eficiência de fluxo até as métricas econômicas que justificam o investimento em infraestrutura agentic.
