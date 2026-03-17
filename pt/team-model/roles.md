---
title: Funções no Desenvolvimento Agêntico
description: >-
  Seis definições de função para a equipe de Desenvolvimento Agêntico — do
  Arquiteto de Contexto ao Arquiteto Principal de Sistemas
authors:
  - dpavancini
lastUpdated: '2026-02-23'
---

## Visão Geral

O desenvolvimento com agentes não elimina os papéis de engenharia — ele os transforma. Títulos tradicionais como "Product Manager" e "QA Engineer" persistem no nome, mas mudam drasticamente na prática. Esta página define seis papéis criados especificamente para o Esquadrão Híbrido, os mapeia a partir de seus equivalentes tradicionais e detalha as responsabilidades diárias e as habilidades que cada um exige.

## Evolução dos Papéis

A transição de papéis tradicionais para papéis com agentes segue um padrão consistente: o esforço humano se afasta da execução direta e se move em direção à especificação, orquestração e avaliação.

| Papel Tradicional | Papel com Agentes | Mudança Principal |
|---|---|---|
| Product Manager | Context Architect | De escrever user stories para engenheirar especificações legíveis por máquina |
| Software Engineer | Agent Operator / Reviewer | De escrever código para orquestrar agentes e auditar a saída |
| QA Engineer | Evaluation Engineer | De encontrar bugs manualmente para projetar sistemas de restrição automatizados |
| Scrum Master | Flow Manager / AgentOps | De facilitar cerimônias para gerenciar o throughput do pipeline de AI |
| DevOps / Platform Engineer | Agent Platform Engineer | De pipelines de CI/CD para ambientes de execução de agentes em sandbox |
| Tech Lead | Principal Systems Architect | De codificação prática para definir leis arquiteturais e amostras de ouro |

Observe que cada mudança move o humano ainda mais a montante — mais perto da intenção, restrições e governança, mais longe da execução linha por linha.

## 1. Context Architect

O Context Architect é o orquestrador estratégico da força de trabalho híbrida. Enquanto um Product Manager tradicional traduz as necessidades de negócio em user stories para desenvolvedores humanos, o Context Architect as traduz em especificações legíveis por máquina que os agentes de AI podem executar sem ambiguidade.

### Funções Principais

- **Elaborar Live Specs** — Produzir documentos de especificação estruturados que incluem critérios de aceitação precisos, edge cases, contratos de input/output e referências de contexto explícitas. Essas specs são a entrada principal para a execução do agente — sua qualidade determina diretamente a qualidade da saída. Esta é a prática de [[context-engineering]] aplicada em nível de equipe.
- **Curar o Context Index** — Manter a base de conhecimento organizada que os agentes utilizam durante a execução: schemas de API, glossários de domínio, logs de decisão, registros de decisão arquitetural e exemplos de implementação anteriores. Um Context Index bem curado reduz a alucinação e o desvio do agente.
- **Configurar HITL gates** — Definir quais tarefas exigem aprovação humana antes do merge e quais podem prosseguir autonomamente. Isso envolve avaliar o risco por tipo de tarefa e definir limites: trabalhos de manutenção de baixo risco fluem automaticamente, enquanto trabalhos de feature de alto risco exigem revisão do Agent Operator.

### Habilidades Essenciais

- **Arquitetura lógica** — A capacidade de decompor requisitos de negócio complexos em especificações estruturadas e inequívocas. Isso está mais próximo da análise de sistemas do que do gerenciamento de produto tradicional.
- **Organização de dados** — Expertise em estruturar conhecimento para consumo por máquina: taxonomias, sistemas de tagging, documentação otimizada para recuperação.
- **Diplomacia com stakeholders** — Traduzir entre stakeholders de negócio que pensam em resultados e sistemas técnicos que exigem inputs precisos. O Context Architect preenche a lacuna entre a intenção humana e a execução da máquina.

## 2. Agent Operator

O Agent Operator é o tech lead de alta alavancagem para o swarm de AI. Ele não passa seus dias escrevendo código de feature do zero. Em vez disso, ele configura, lança, monitora e audita as execuções de agentes — intervindo apenas quando os agentes travam ou produzem uma saída abaixo do padrão.

### Funções Principais

- **Desbloquear agentes travados** — Diagnosticar por que um agente entrou em um loop, interpretou mal uma spec ou falhou em produzir testes aprovados. O Agent Operator fornece contexto corretivo, ajusta parâmetros e relança. Esta é a "Agent Recovery" — a parte mais sensível ao tempo do papel.
- **Escrever scripts e tooling personalizados** — Construir a automação que conecta agentes ao ambiente de desenvolvimento: servidores MCP personalizados, scripts de injeção de contexto e dashboards de monitoramento.
- **Escrever código crítico "Human-Owned Core"** — Alguns códigos são importantes demais ou arquiteturalmente sensíveis demais para a execução do agente. O Agent Operator escreve esses componentes manualmente — caminhos críticos de segurança, algoritmos centrais e abstrações fundamentais das quais todo o resto depende.

### Habilidades Essenciais

- **Auditoria de código** — A capacidade de revisar rapidamente o código gerado por agentes quanto à correção, vulnerabilidades de segurança, problemas de desempenho e conformidade arquitetural. Isso exige ler código mais rápido do que escrevê-lo.
- **Depuração Linux/sistema** — Agentes operam em ambientes sandboxed. Quando algo quebra no nível da infraestrutura — permissões de arquivo, políticas de rede, limites de recursos — o Agent Operator precisa diagnosticar e corrigir.
- **Prompt engineering técnico** — Criar os prompts, instruções de sistema e pacotes de contexto que direcionam o comportamento do agente. Isso não se trata de palavras inteligentes — trata-se de fornecer as informações certas na estrutura certa para o modelo raciocinar de forma eficaz.

## 3. Evaluation Engineer

O Evaluation Engineer representa a transformação de papel mais dramática na equipe com agentes. QA engineers tradicionais encontram bugs testando software manualmente. Evaluation Engineers mudam o QA de encontrar bugs para projetar as restrições que impedem que eles sejam criados em primeiro lugar.

### Funções Principais

- **Construir ambientes de teste Dockerized** — Criar ambientes isolados e reproduzíveis onde os agentes executam e sua saída é validada. Esses ambientes espelham as condições de produção, mantendo um isolamento rigoroso para segurança.
- **Escrever casos de teste antes do agente começar** — Definir os critérios de avaliação antecipadamente, não depois. O Evaluation Engineer produz test harnesses que o agente deve satisfazer, transformando o QA de um checkpoint reativo em uma especificação proativa.
- **Desenvolver rubricas LLM-as-a-Judge** — Criar frameworks de avaliação estruturados onde um LLM secundário avalia a saída do agente em relação a critérios de qualidade: aderência ao estilo de código, padrões de segurança, completude da documentação e conformidade arquitetural. Essas rubricas automatizam os aspectos da revisão de código que não exigem julgamento humano.

### Habilidades Essenciais

- **Python/TypeScript** — Linguagens primárias para escrever harnesses de avaliação, geradores de teste e scripts de automação.
- **Containerization** — Profunda expertise em Docker e orquestração de contêineres para construir os ambientes isolados onde os agentes executam com segurança.
- **Análise estatística** — Avaliar o desempenho do agente requer pensamento estatístico: rastrear taxas de aprovação ao longo do tempo, identificar padrões de regressão, medir intervalos de confiança em métricas de qualidade e distinguir sinal de ruído nos resultados da avaliação.

## 4. Flow Manager

O Flow Manager é o "Controlador de Tráfego Aéreo" do Esquadrão Híbrido. Enquanto um Scrum Master tradicional facilita cerimônias humanas e remove impedimentos, o Flow Manager garante que todo o pipeline — da criação de especificações à execução do agente e à revisão humana — flua sem gargalos.

### Funções Principais

- **Observabilidade do pipeline** — Monitorar o fluxo de trabalho de ponta a ponta na squad: quantas specs estão em refinamento, quantos agentes estão executando, quantos pull requests aguardam revisão e onde as filas estão se formando. O Flow Manager detecta gargalos antes que eles se espalhem.
- **Gerenciamento do orçamento de computação de AI (Agent FinOps)** — Rastrear e otimizar o custo da execução do agente. Cada execução de agente consome tokens de API, recursos de computação e tempo de infraestrutura. O Flow Manager equilibra o throughput com o custo, garantindo que a squad entregue o máximo valor por dólar gasto. Isso se conecta diretamente às práticas de [[llmops]] aplicadas em nível de equipe.
- **Balanceamento de carga de revisões** — Distribuir o trabalho de revisão entre os Agent Operators para evitar que uma única pessoa se torne um gargalo. Quando as filas de revisão aumentam, o Flow Manager redistribui o trabalho ou escala problemas de capacidade.

### Habilidades Essenciais

- **Teoria Lean/Kanban** — Compreensão do gerenciamento de trabalho baseado em fluxo: limites de trabalho em andamento, medição de cycle time, otimização de throughput e teoria de gargalos. Esses conceitos se traduzem diretamente no gerenciamento de pipelines híbridos humano-agente.
- **Letramento em dados** — Conforto com dashboards, métricas e análise de tendências. O Flow Manager vive em ferramentas de observabilidade e precisa interpretar rapidamente o que os dados estão dizendo sobre o desempenho da squad.
- **Consciência financeira** — Compreender o modelo de custo da execução do agente de AI: precificação de tokens, custos de computação e o cálculo de ROI que justifica o uso do agente para diferentes tipos de tarefa.

## 5. Agent Platform Engineer

O Agent Platform Engineer constrói e mantém o Workbench Runtime — o ambiente de execução sandboxed onde os agentes operam. Este papel é o equivalente com agentes de um DevOps/Platform Engineer, mas focado na infraestrutura de agentes em vez da infraestrutura de aplicações.

### Funções Principais

- **Infraestrutura de microVM isolada** — Construir os ambientes seguros e sandboxed onde os agentes executam o código. Cada execução de agente opera em seu próprio ambiente isolado com acesso controlado ao filesystem, rede e recursos do sistema. Isso evita que os agentes afetem os sistemas de produção ou uns aos outros.
- **JSON Function Definitions** — Definir as interfaces de ferramentas que os agentes usam para interagir com o ambiente de desenvolvimento. Cada chamada de ferramenta que um agente pode fazer — leituras de arquivo, execução de comandos, chamadas de API — deve ser explicitamente definida, documentada e protegida.
- **Governança de egress de rede** — Controlar o que os agentes podem acessar pela rede. Por padrão, os agentes devem ter acesso mínimo à rede. O Agent Platform Engineer define e impõe allowlists para endpoints de API, registros de pacotes e fontes de documentação.

### Habilidades Essenciais

- **Virtualização em nível de kernel** — Profundo entendimento de namespaces Linux, cgroups, perfis seccomp e runtimes de contêiner. O sandboxing de agentes requer controle granular sobre o que os processos podem fazer no nível do sistema operacional.
- **eBPF** — Expertise em extended Berkeley Packet Filter para observabilidade em tempo de execução e aplicação de segurança. eBPF fornece o monitoramento de baixo overhead necessário para rastrear o comportamento do agente sem impactar o desempenho da execução.
- **Design de API** — Projetar as interfaces de ferramentas que os agentes consomem. Essas APIs devem ser precisas, bem documentadas e à prova de falhas — porque o consumidor é um modelo de AI que não pode fazer perguntas de esclarecimento quando uma interface é ambígua.

## 6. Principal Systems Architect

O Principal Systems Architect define as "Leis da Física" para a codebase. Enquanto um Tech Lead tradicional pode dividir o tempo entre codificação e orientação técnica, o Principal Systems Architect se concentra quase inteiramente na definição de restrições — criando as regras e materiais de referência que garantem que o código gerado por agentes seja estruturalmente sólido.

### Funções Principais

- **Contextos delimitados (DDD)** — Definir limites de domínio claros usando princípios de Domain-Driven Design. Cada contexto delimitado tem sua própria linguagem, modelos e regras. Agentes operando dentro de um contexto delimitado herdam essas restrições, o que os impede de criar acoplamento entre domínios ou violar a separação de preocupações.
- **Testes automatizados de restrição arquitetural** — Escrever testes executáveis que verificam regras arquiteturais: verificações de direção de dependência, detecção de violação de camada, aplicação de convenção de nomenclatura e validação de contrato de API. Esses testes são executados como parte do pipeline de execução do agente, capturando violações antes que o código chegue à revisão.
- **Curadoria de Golden Samples** — Selecionar e manter implementações de referência que demonstram a maneira correta de construir cada tipo de componente na codebase. Golden Samples são a forma mais eficiente de instrução para agentes de AI — um exemplo bem escolhido comunica padrões, estilo e intenção de forma mais eficaz do que páginas de regras escritas.

### Habilidades Essenciais

- **Domínio de DDD** — Fluência em Domain-Driven Design: bounded contexts, aggregates, domain events, anti-corruption layers e context mapping. Esses conceitos fornecem o vocabulário para definir os limites que os agentes devem respeitar.
- **Ferramentas de análise estática** — Expertise em ferramentas como ArchUnit, dependency-cruiser, regras personalizadas do ESLint e frameworks semelhantes que codificam regras arquiteturais como verificações executáveis.
- **Refactoring de legado** — Experiência com a modernização incremental de grandes codebases. O Principal Systems Architect deve definir caminhos de migração que os agentes podem executar com segurança, um contexto delimitado por vez.

## Dimensionando a Squad

Nem toda equipe precisa de todos os seis papéis desde o primeiro dia. A composição da squad escala com a maturidade da sua adoção de agentes:

- **Começando (1-2 agentes):** Um único engenheiro atua como Agent Operator e Context Architect. O tech lead existente assume as responsabilidades de Principal Systems Architect. Nenhum Flow Manager ou Platform Engineer dedicado é necessário ainda.
- **Crescendo (3-5 agentes):** Surgem papéis dedicados de Context Architect e Agent Operator. Um engenheiro com experiência em DevOps começa a construir o Workbench Runtime. O QA começa a transitar para o modelo de Evaluation Engineer.
- **Em escala (5+ agentes):** Todos os seis papéis são preenchidos. O Flow Manager se torna essencial à medida que a complexidade do pipeline cresce. O Agent Platform Engineer mantém uma infraestrutura de sandboxing cada vez mais sofisticada.

O princípio chave: adicione papéis conforme a dor aparece, não por antecipação. Deixe que os gargalos digam quando um novo papel é necessário.

## O Que Vem a Seguir

Com os papéis e a estrutura da equipe definidos, o próximo passo é construir a infraestrutura sobre a qual essas equipes operam — os ambientes de execução sandboxed, sistemas de observabilidade e frameworks de governança que tornam a execução segura e escalável de agentes possível.
