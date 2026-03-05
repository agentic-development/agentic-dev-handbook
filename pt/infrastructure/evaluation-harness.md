---
title: O Arnês de Avaliação
description: >-
  Testes automatizados, portões de qualidade e mecanismos de governança que
  validam código gerado por agente
authors:
  - dpavancini
lastUpdated: '2026-02-23'
---

## Visão Geral

O Evaluation Harness é o pacote de testes automatizado que roda continuamente durante a execução do agent, validando cada saída antes que ela chegue a um revisor humano. Ele combina testes funcionais, varreduras de segurança, verificações de conformidade arquitetônica e avaliações LLM-as-a-Judge em um gate de qualidade unificado. Nenhum código gerado por agent é apresentado a um humano até que passe pelo Eval Harness.

## Por Que o Eval Harness Existe

No desenvolvimento tradicional, a garantia de qualidade acontece após a implementação: um desenvolvedor escreve o código, e então testadores e revisores o verificam. No desenvolvimento agentic, essa sequência é invertida. O Eval Harness define os critérios de aceitação antes que o agent comece, executa a validação continuamente durante a execução e restringe a saída antes que ela entre na fila de revisão.

Essa inversão serve a dois propósitos:

1. **Torna a execução autônoma confiável.** Humanos não podem revisar cada linha que um agent produz na velocidade em que os agents as produzem. O Eval Harness atua como um primeiro revisor automatizado que detecta a maioria dos problemas, para que os humanos possam focar sua atenção nos casos que exigem julgamento.
2. **Cria um ciclo de feedback.** Quando a saída de um agent falha na avaliação, o harness retorna feedback estruturado que o agent usa para corrigir seu trabalho e tentar novamente. Esse ciclo continua até que a saída passe ou o orçamento de execução seja esgotado.

## Dois Tipos de Validação

Nem toda validação é igual. O Eval Harness distingue entre dois tipos fundamentalmente diferentes de verificações, cada um adequado a diferentes aspectos da qualidade do código.

### Validação Determinística

A validação determinística produz resultados binários de aprovação/reprovação com base em regras estritas e inequívocas. Não há área cinzenta — a verificação é aprovada ou não.

Exemplos de verificações determinísticas:

- **Forbidden imports** — O código do agent importa um módulo que é proibido pelas regras arquitetônicas do projeto. A verificação bloqueia o PR automaticamente.
- **Type safety** — O compilador TypeScript relata erros de tipo. O código não compila.
- **Security scans** — A análise estática detecta um padrão de vulnerabilidade conhecido, como SQL injection ou credenciais hardcoded.
- **Test suite passage** — Testes de unidade e integração devem todos passar. Uma única falha bloqueia a saída.
- **Linting and formatting** — O código deve estar em conformidade com as regras de estilo do projeto. Código não-conforme é rejeitado.
- **Dependency policy** — Novas dependencies devem estar na lista aprovada. Pacotes não aprovados são sinalizados.

Verificações determinísticas são rápidas, confiáveis e baratas. Elas devem cobrir todas as regras que podem ser expressas como uma condição binária. Quando uma regra pode ser tornada determinística, ela deve ser — não deixe decisões determinísticas para avaliação probabilística.

### Avaliação Probabilística

A avaliação probabilística usa um LLM-as-a-Judge para avaliar aspectos não determinísticos da saída do agent que não podem ser reduzidos a regras binárias. Essas avaliações produzem scores, grades ou avaliações ranqueadas em vez de simples resultados de aprovação/reprovação.

Exemplos de avaliações probabilísticas:

- **Code quality grading** — Um modelo judge avalia se o código gerado segue padrões idiomáticos, usa nomes de variáveis significativos e mantém níveis de abstração apropriados.
- **Documentation quality** — O judge avalia se a documentação gerada é clara, precisa e completa o suficiente para o público-alvo.
- **Architectural alignment** — Para casos que vão além do que a análise estática pode verificar, o judge avalia se a implementação se alinha com a intenção arquitetônica do projeto.
- **Tone and style** — Para comunicações geradas por agent (commit messages, PR descriptions, user-facing text), o judge avalia se o tom corresponde aos padrões organizacionais.

As avaliações probabilísticas são configuradas com rubrics — critérios de pontuação estruturados que guiam o modelo judge. Um rubric define as dimensões sendo avaliadas, a escala e o threshold para aprovação.

### Combinando Ambos os Tipos

O Eval Harness mais eficaz combina ambos os tipos de validação em uma abordagem em camadas:

1. **As verificações determinísticas são executadas primeiro.** Elas são rápidas e baratas. Se o código falha em uma verificação determinística, não faz sentido executar avaliações probabilísticas caras.
2. **As avaliações probabilísticas são executadas em segundo lugar.** Elas avaliam os aspectos que sobrevivem à filtragem determinística — as qualidades que exigem julgamento em vez de regras.

Essa abordagem em camadas otimiza tanto a confiabilidade quanto o custo. As verificações determinísticas detectam os problemas fáceis rapidamente; as avaliações probabilísticas detectam os mais sutis.

### Armadilha de Implementação: Confiança Excessiva na Avaliação Probabilística

Um erro comum e perigoso é confiar apenas em avaliações probabilísticas de LLM para lógica de missão crítica. LLM judges são poderosos, mas não determinísticos — eles podem produzir diferentes avaliações para o mesmo input em diferentes execuções, e podem ser enganados por código escrito com confiança, mas incorreto.

A regra geral: se uma verificação pode ser expressa como uma regra determinística, ela deve ser determinística. Reserve a avaliação probabilística para qualidades que realmente exigem julgamento. Nunca use um LLM judge como o único gate para validação crítica de segurança, conformidade ou segurança. Combine restrições de código determinísticas com verificações de raciocínio probabilístico para obter confiabilidade e nuance.

## Governança

O Eval Harness é um componente de um sistema de governança mais amplo que garante que o código gerado por agent atenda aos padrões organizacionais de segurança, conformidade e qualidade.

### [[human-in-the-loop|Human-in-the-Loop]] Gates

Algumas decisões são muito importantes para automação total, independentemente de quão sofisticado o Eval Harness se torne. Os gates HITL obrigatórios incluem:

- **Mudanças críticas de segurança** — Fluxos de autenticação, lógica de autorização, implementações de criptografia e qualquer código que lide com dados sensíveis.
- **Decisões arquitetônicas** — Novas fronteiras de serviço, alterações de schema de banco de dados, modificações de contrato de API e seleção de tecnologia.
- **Deployments de alto raio de impacto** — Mudanças que afetam todos os usuários, infraestrutura crítica ou sistemas financeiros.
- **Padrões inovadores** — Implementações pioneiras de abordagens não cobertas por especificações existentes ou Golden Samples.

O Eval Harness sinaliza essas mudanças para revisão humana automaticamente com base em regras definidas pelo Context Architect e Principal Systems Architect. A percepção chave é que nem todas as mudanças de código carregam o mesmo risco. Os gates HITL alocam a atenção humana onde ela é mais importante.

### [[guardrails|Guardrails]] Integration

O Eval Harness se integra ao sistema guardrails mais amplo para aplicar restrições em vários níveis:

- **Input guardrails** — Validam a especificação e o context antes que o agent inicie a execução. Rejeitam especificações malformadas, sinalizam context ausente e verificam padrões de conflito conhecidos.
- **Runtime guardrails** — Monitoram o agent durante a execução. Intervêm se o agent tentar operações proibidas, exceder limites de recursos ou entrar em loops improdutivos.
- **Output guardrails** — Validam os artefatos finais antes que saiam do workbench. É aqui que as verificações determinísticas e probabilísticas do Eval Harness são executadas.

## Observabilidade

Governança sem visibilidade é governança apenas no nome. O Eval Harness produz traces de execução estruturados que tornam cada decisão do agent auditável e debuggable.

### Execution Traces

Cada execução do agent produz um trace que vincula a cadeia de raciocínio completa:

- **Thought** — O que o agent decidiu fazer e por quê (o raciocínio do modelo)
- **Action** — Qual tool call, mudança de código ou comando o agent executou
- **Result** — O que aconteceu: a saída da tool, resultados de testes ou mensagens de erro

Esses traces são armazenados junto com os artefatos do agent e são acessíveis durante a code review. Quando um revisor quer entender por que o agent fez uma determinada escolha de implementação, o trace fornece o caminho de raciocínio completo.

### [[llmops|LLMOps]] Dashboards

Agrega traces de execução em dashboards operacionais que mostram:

- **Pass rates** — Qual porcentagem de execuções de agent passa pelo Eval Harness na primeira tentativa? Taxas de aprovação em declínio sinalizam degradação de context ou problemas de qualidade da especificação.
- **Failure categories** — Quais tipos de verificações falham com mais frequência? Isso identifica onde investir em melhor context, melhores especificações ou melhores tools.
- **Cycle times** — Quanto tempo leva desde o dispatch da tarefa até a passagem pelo Eval Harness? O aumento dos cycle times pode indicar que as tarefas são muito complexas para execução por um único agent.
- **Retry patterns** — Quantos loops de avaliação a tarefa média exige? Tarefas que consistentemente precisam de muitas retries podem precisar de melhores especificações ou decomposição diferente.

### FinOps e Token Budget Management

Cada execução de agent consome API tokens, recursos de computação e tempo de infraestrutura. Sem controles de orçamento, um único agent travado pode consumir recursos significativos antes que alguém perceba.

O Eval Harness integra o gerenciamento de orçamento de token através de circuit breakers:

- **Per-task budgets** — Cada tarefa tem um orçamento máximo de token. Quando o orçamento é esgotado, o agent para e a tarefa é escalada para um humano.
- **Per-loop limits** — O número máximo de loops de avaliação-retry é limitado. Um agent que não consegue passar pelo harness em um número razoável de tentativas provavelmente não terá sucesso com mais tentativas.
- **Cost tracking** — Cada execução de agent registra seu custo total: model tokens, tool invocations, compute time. Esses dados alimentam os FinOps dashboards do Flow Manager para gerenciamento de orçamento em nível de equipe.
- **Circuit breakers** — Se o custo por tarefa de um agent exceder um threshold configurável, o circuit breaker desarma e interrompe a execução. Isso evita custos excessivos de agents presos em loops improdutivos.

Os controles de orçamento não são apenas sobre economia de custos. Eles são um sinal de qualidade. Tarefas que consistentemente esgotam seus orçamentos são tarefas onde algo está errado — a especificação é pouco clara, o context é insuficiente ou a tarefa está além da capacidade atual de execução autônoma.

## Construindo Seu Eval Harness

### Comece Simples

O mínimo viável do Eval Harness consiste em:

1. **Seu pacote de testes existente** — Testes de unidade, testes de integração e testes end-to-end que você já possui.
2. **Um linter e formatter** — Impõem o estilo do código de forma determinística.
3. **Um security scanner** — Executa análise estática para padrões de vulnerabilidade conhecidos.
4. **Um gate de aprovação/reprovação** — Bloqueia a saída do agent que falha em qualquer um dos itens acima.

Esta linha de base detecta os erros mais comuns do agent e estabelece o hábito da validação automatizada. Você pode adicionar avaliações probabilísticas, verificações arquitetônicas e governança sofisticada mais tarde.

### [[test-generation-workflow|Test Generation]] Integration

Agents podem ajudar a construir o harness que avalia sua própria saída. Um padrão poderoso é ter um agent gerando testes a partir do Behavioral Contract da especificação e um agent diferente gerando a implementação. O agent que escreve os testes e o agent de implementação operam independentemente, reduzindo o risco de ambos cometerem o mesmo erro.

Essa abordagem também dimensiona o Eval Harness organicamente. À medida que a equipe escreve mais especificações com Behavioral Contracts mais detalhados, a cobertura de testes do harness cresce automaticamente.

### Maturidade da Progressão

À medida que sua prática agentic amadurece, o Eval Harness evolui:

1. **Nível 1: Existing checks** — Executa seu pacote de testes e linters atuais contra a saída do agent.
2. **Nível 2: Spec-derived checks** — Gera testes automaticamente a partir dos critérios de aceitação da Live Spec.
3. **Nível 3: LLM-as-a-Judge** — Adiciona avaliações probabilísticas para qualidade de código, documentação e alinhamento arquitetônico.
4. **Nível 4: Continuous governance** — Integra gates HITL, controles de custo e observabilidade em um pipeline de governança unificado.
5. **Nível 5: Self-improving** — O harness usa dados de execução para identificar lacunas em sua própria cobertura e sugere novas verificações ao Evaluation Engineer.

Cada nível constrói sobre o anterior. Não pule etapas — um LLM judge sofisticado é inútil se seu pacote de testes básico não roda de forma confiável.

## O Que Vem a Seguir

O Agent Workbench, Context Management e Evaluation Harness juntos formam a infraestrutura operacional do desenvolvimento agentic. Com esta infraestrutura implementada, o próximo passo é construir a equipe que a opera — os papéis, habilidades e estruturas organizacionais abordados no capítulo Team Model.
