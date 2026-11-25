# Gestão Estratégica do Reembolso Assistencial na Saúde Suplementar

> **Aplicação Completa do Modelo CRISP-DM para Mitigação de Custos Reparadores (RN 566/22) e Proteção do IDSS**

![Metodologia](https://img.shields.io/badge/Metodologia-CRISP--DM-0A66C2?style=flat-square)
![Ferramenta](https://img.shields.io/badge/BI-Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black)
![Regulação](https://img.shields.io/badge/ANS-RN%20566%2F22-1F883D?style=flat-square)
![Série Histórica](https://img.shields.io/badge/Série%20Histórica-2024%20a%202026-555555?style=flat-square)

---

## Informações do Projeto

| Campo | Detalhes |
| :--- | :--- |
| **Autores / Analistas** | Júlio César de Aguiar Nunes (RGM: 33282081)<br>Eduardo Salgueiro Evangelista (RGM: 35856653) |
| **Instituição / Curso** | Centro Universitário do Distrito Federal (UDF) — Sistemas de Informação |
| **Disciplina / Docente** | Ciência de Dados e Aprendizagem de Máquina — Profa. Kadidja Valéria |
| **Data / Referência** | Novembro de 2026 |
| **Série Histórica Analisada** | 2024 a 2026 |

---

## Sumário

1. [Síntese Executiva dos Indicadores-Chave (2024–2026)](#síntese-executiva-dos-indicadores-chave-20242026)
2. [Fase 1: Entendimento do Negócio](#fase-1-entendimento-do-negócio)
3. [Fase 2: Entendimento dos Dados](#fase-2-entendimento-dos-dados)
4. [Fase 3: Preparação dos Dados](#fase-3-preparação-dos-dados)
5. [Fase 4: Modelagem](#fase-4-modelagem)
6. [Fase 5: Avaliação](#fase-5-avaliação)
7. [Fase 6: Implementação](#fase-6-implementação)

---

## Síntese Executiva dos Indicadores-Chave (2024–2026)

| Total de Solicitações | Solicitações Pagas | Taxa de Não Aprovação | Média de Pagamento |
| :---: | :---: | :---: | :---: |
| **150.673** | **75.100** | **50,79%** | **24 dias** |
| R$ 124,05 Mi solicitados | R$ 83,10 Mi pagos | vs. 49,21% concluídas | 42,84% > 18d em Set/26 |

Este relatório consolida a pesquisa de negócios e dados aplicada ao processo de reembolso assistencial de uma operadora de planos de saúde, seguindo rigorosamente as seis fases iterativas do modelo **CRISP-DM** (*Cross-Industry Standard Process for Data Mining*). A análise integra as diretrizes regulatórias da Agência Nacional de Saúde Suplementar (**ANS**) aos indicadores extraídos dos painéis gerenciais em **Power BI**, transformando dados transacionais em recomendações estratégicas para redução de custos reparadores, eficiência operacional e proteção do **IDSS**.

---

## Fase 1: Entendimento do Negócio

> **Objetivo Central da Fase 1 (CRISP-DM):** Traduzir o problema regulatório e financeiro da operadora em metas técnicas viáveis antes de investir recursos computacionais, alinhando expectativas entre as áreas de negócio e a equipe técnica de dados.

### 1.1 Contexto Regulatório e Problema de Negócio

A **Resolução Normativa (RN) nº 566/22** da ANS regulamenta a garantia de atendimento dos beneficiários de planos privados de assistência à saúde e estabelece prazos máximos obrigatórios para consultas, exames e procedimentos. Quando a operadora não consegue assegurar o atendimento dentro da rede credenciada no prazo legal, a própria norma prevê a medida reparadora por meio do reembolso integral ou parcial ao beneficiário.

Embora funcione como válvula de escape regulatória, a recorrência de reembolsos por falha de rede exerce peso negativo direto no **Índice de Desempenho da Saúde Suplementar (IDSS)**. Operadoras avaliadas na Faixa 1 (menor pontuação) ficam expostas a fiscalizações rigorosas, sanções administrativas (multas), regimes de direção fiscal ou técnica e, em cenários extremos, interrupção das atividades com migração compulsória (portabilidade extraordinária) da carteira de beneficiários.

### 1.2 Objetivos de Negócio e Critérios de Sucesso

* **Sustentabilidade Financeira:** Mitigar o avanço do gasto assistencial decorrente de medidas reparadoras, mantendo a sinistralidade/custo assistencial em patamar saudável (meta de até 85%).
* **Conformidade Regulatória e IDSS:** Reduzir o volume de guias pagas fora do prazo legal (30 dias) e do prazo interno de análise (18 dias), prevenindo Notificações de Intermediação Preliminar (NIPs) e preservando a nota do IDSS.
* **Suporte Estratégico Multissetorial:** Subsidiar com evidências os setores de Credenciamento, Sinistro, Assessoria de Qualidade, Compliance, Jurídico e Assessoria de Comunicação junto à ANS, revelando fragilidades regionais da rede credenciada e desvios de comportamento de prestadores.

### 1.3 Avaliação da Situação e Metas Técnicas de Mineração de Dados

* **Recursos e Fontes Disponíveis:** Base histórica de solicitações e pagamentos de reembolso (2024 a 2026) estruturada em modelo Power BI (`Reembolso Geral.pbix`) com recortes por Unidade Administrativa (UA/UF), Tipo de Reembolso, Forma de Reembolso, Canal, CIB e datas de ciclo de vida da guia.
* **Riscos Identificados:** Subnotificação de causas clínicas (ausência de código TUSS/especialidade nos painéis atuais), distorção de médias por *outliers* temporais e risco de decisões precipitadas baseadas em meses com corte incompleto.
* **Metas Técnicas:** Consolidar, auditar e disponibilizar um ecossistema de painéis gerenciais capaz de rastrear volume, valor financeiro, taxa de aprovação, mix de canais e aderência aos SLAs de 18 e 30 dias, preparando a arquitetura de dados para futuras etapas de clusterização e modelagem preditiva.

---

## Fase 2: Entendimento dos Dados

> **Objetivo Central da Fase 2 (CRISP-DM):** Acessar, quantificar e qualificar a matéria-prima disponível para levantar hipóteses iniciais e evitar falhas imprevistas nas etapas de preparação e modelagem.

### 2.1 Descrição Quantitativa e Exploração dos Indicadores

A exploração inicial dos quatro painéis analíticos revela um volume transacional elevado entre 2024 e 2026, com forte aceleração financeira em meados de 2026 e gargalos operacionais concentrados na etapa de análise e em estados específicos.

| Dimensão Analisada | Métricas Observadas | Diagnóstico de Negócio |
| :--- | :--- | :--- |
| **Volume Total Solicitado** | 150.673 guias \| R$ 124.051.306 | Alta demanda recorrente exigindo governança de rede e triagem automatizada. |
| **Volume Efetivamente Pago** | 75.100 guias \| R$ 83.102.198 | 49,8% das guias concentram 67,0% do valor solicitado (ticket médio pago superior ao indeferido: $\approx$ R$ 1.106 vs. $\approx$ R$ 542). |
| **Taxa de Aprovação (Status)** | 49,21% Concluído vs. 50,79% Não Aprovada | Mais da metade das entradas é indeferida, gerando alto retrabalho operacional e atrito com o beneficiário. |
| **Canais de Entrada** | Internet (52,89%), Atendimento (20,63%), App (19,25%), Outros (7,23%) | 72,14% das demandas entram por canais digitais, criando oportunidade direta para filtros preventivos no envio. |
| **Tendência Financeira (2026)** | Picos em Jun/26 (R$ 4,0 Mi), Jul/26 (R$ 3,7 Mi) e Set/26 (R$ 3,7 Mi) | Crescimento acentuado do desembolso no 2º e 3º trimestres de 2026 frente a 2024 e 2025. |
| **SLA de Pagamento (30 dias)** | Média: 24 dias \| Picos fora do prazo em Mar (2.745) e Abr (3.308) | Média global mascara atrasos críticos: casos extremos atingem 187 dias (RR) e 126–160 dias (SE). |
| **SLA de Análise (18 dias)** | Set/2026: 1.677 até 18d (57,16%) vs. 1.257 > 18d (42,84%) | Quase metade das guias estoura o prazo interno de análise, com UFs (PR, RR, TO, AC, AP, BA) levando 28 a 30 dias. |

* **Figura 1:** *Comparativo interanual do valor pago em reembolsos por mês (2024–2026). Fonte: Elaboração própria a partir dos painéis Power BI.*
  > *(Dica: caso publique em um repositório, adicione a imagem em `docs/img/figura1_valor_pago_mes.png` e referencie-a aqui.)*
* **Figura 2:** *Distribuição de solicitações por canal de entrada e indicadores de aprovação e SLA de análise (18 dias). Fonte: Elaboração própria.*
  > *(Dica: caso publique em um repositório, adicione a imagem em `docs/img/figura2_canais_e_sla.png` e referencie-a aqui.)*

### 2.2 Verificação de Qualidade dos Dados (Anomalias e Inconsistências)

* **Corte Temporal e Incompletude (Out/Nov 2026):** Há queda abrupta nas solicitações de outubro/2026 (89 guias) e nos pagamentos de outubro (796 guias / R$ 0,8 Mi) e novembro/2026 (42 guias / R$ 0,0 Mi). Trata-se de meses em processamento ou janela de extração parcial que precisam ser sinalizados ou filtrados em séries temporais para não induzir interpretações equivocadas de redução de sinistro.
* **Outliers Temporais Severos:** A Tabela de Controle de Guias Pagas registra atrasos extremos, como a guia `2366735778` (RR) com 187 dias (26/01/2024 a 31/07/2024) e múltiplos registros em Sergipe (SE) entre 126 e 160 dias no primeiro semestre de 2026. Tais registros distorcem a média aritmética e exigem validação quanto a judicialização, pendência documental ou erro de baixa sistêmica.
* **Consistência de Totais e Filtros:** A soma das guias pagas (75.100) representa 49,84% do total de solicitações (150.673), apresentando leve variação frente aos 49,21% exibidos no gráfico de status (diferença atribuível a guias em trânsito/processamento entre o status de aprovação e a efetiva liquidação financeira).

---

## Fase 3: Preparação dos Dados

> **Objetivo Central da Fase 3 (CRISP-DM):** Transformar, limpar e estruturar os dados brutos em um formato confiável para análise e modelagem. Conforme o Guia CRISP-DM, esta fase consome entre 50% e 70% do tempo e esforço total do projeto.

A preparação dos dados foi estruturada nos quatro macroprocessos definidos pela metodologia (**Selecionar**, **Limpar**, **Construir** e **Integrar**), garantindo rastreabilidade de ponta a ponta desde a entrada da guia até o pagamento:

| Etapa | Atividades Executadas e Regras de Negócio Aplicadas |
| :--- | :--- |
| **1. Selecionar (Filtrar)** | Definição das variáveis-chave de negócio: Unidade Administrativa (UA/UF), Número da Guia, Tipo de Reembolso, Forma de Reembolso, Canal de Entrada, CIB, Datas (Solicitação, Análise e Finalização), Status e Valores (Solicitado e Pago). Exclusão de atributos redundantes ou confidenciais desnecessários à visão gerencial (aderência à LGPD). |
| **2. Limpar (Tratar)** | Padronização categórica dos status (`Pagamento Concluído` vs. `Solicitação Não Aprovada`), tratamento de datas nulas para guias ainda em aberto, deduplicação por Número da Guia e criação de *flag* de corte temporal para isolar meses incompletos (outubro e novembro de 2026). |
| **3. Construir (Derivar)** | Engenharia de atributos voltada aos prazos da ANS e SLAs internos: cálculo de `Qtd. Dias Análise` ($\text{Data Análise} - \text{Data Solicitação}$), classificação `SLA Análise 18 Dias` (`Até 18 Dias` / `Acima de 18 Dias`), cálculo de `Qtd. Dias Finalização` ($\text{Data Finalização} - \text{Data Solicitação}$), indicador binário `Finalizado em 30 Dias` (`SIM` / `NÃO`) e `Média de Pagamento em Dias`. |
| **4. Integrar (Mesclar)** | Unificação das tabelas transacionais de solicitações, fila de análise técnica e liquidação financeira em um modelo relacional estrela (*Star Schema*) no Power BI, com dimensão calendário única habilitando filtros cruzados entre volume, desembolso e cumprimento de prazo. |

---

## Fase 4: Modelagem

> **Objetivo Central da Fase 4 (CRISP-DM):** Aplicar técnicas analíticas e matemáticas sobre os dados preparados para extrair padrões de alto valor e sustentar decisões estratégicas e operacionais.

### 4.1 Modelagem Analítica e Diagnóstica (Entregue no Power BI)

Na primeira iteração do ciclo, a modelagem foi direcionada à inteligência descritiva e diagnóstica, consolidando quatro camadas visuais integradas que atendem desde a diretoria executiva até a operação de auditoria:

1. **Painel 1 — Visão Geral de Solicitações:** Monitora demanda total (150.673 guias / R$ 124,05 Mi), taxa de aprovação (49,21% vs. 50,79%) e sazonalidade interanual (2024–2026).
2. **Painel 2 — Controle de SLA de Análise (18 Dias):** Mapeia gargalos na etapa de auditoria médica/administrativa, expondo guias críticas por UF (em set/2026: 1.257 guias acima de 18 dias, com casos de 28 a 30 dias em PR, RR, TO, AC, AP e BA).
3. **Painel 3 — Controle de SLA de Pagamento (30 Dias):** Rastreia guias finalizadas dentro e fora do prazo legal mês a mês (picos de atraso em março com 2.745 e abril com 3.308 guias) e gera tabela operacional para exportação em Excel e atuação corretiva.
4. **Painel 4 — Pagamento de Reembolso (Visão Financeira):** Acompanha desembolso mensal em milhões de reais (R$ 83,10 Mi acumulados), média de pagamento (24 dias) e representatividade por canal de entrada.

### 4.2 Evolução para Modelagem Preditiva e Clusterização (Próxima Iteração)

Para evoluir a operadora de uma gestão reativa para uma atuação preventiva, recomenda-se aplicar na próxima volta do ciclo CRISP-DM duas famílias de algoritmos de Aprendizagem de Máquina (com partição treino/teste em 70%/30% e validação cruzada):

* **Clusterização (`K-Means` / `DBSCAN`):** Segmentar Unidades Administrativas (UFs) e tipos de reembolso segundo reincidência, ticket médio e tempo de ciclo, isolando *clusters* críticos de falha de rede (como SE e RR) para priorização do Credenciamento.
* **Classificação Supervisionada (`Random Forest` / `XGBoost`):** Estimar, no instante do protocolo da guia, a probabilidade de (a) estouro do SLA de 18/30 dias e (b) indeferimento da solicitação. Guias com alto risco de atraso e alta probabilidade de aprovação passam a receber priorização automática na fila de análise.

---

## Fase 5: Avaliação

> **Princípio-Chave da Fase 5 (CRISP-DM): Impacto Real > Precisão Técnica.** Os resultados são avaliados exclusivamente sob a lente dos critérios de sucesso do negócio definidos na Fase 1, verificando se respondem à dor estratégica da operadora e definindo se o projeto segue para implementação ou exige nova iteração.

### 5.1 Confronto com as Metas de Negócio

* **Aderência às Metas:** O modelo analítico atinge integralmente o objetivo de dar transparência ao custo reparador (R$ 83,10 Mi pagos) e aos pontos exatos de ruptura de prazo. A diretoria passa a enxergar a escalada de custos do 2º e 3º trimestres de 2026 e os estados ofensores de SLA.
* **Achado Crítico 1 — Retrabalho Operacional (50,79%):** Mais da metade das solicitações é não aprovada. Isso consome horas preciosas da equipe de análise, gerando efeito cascata que empurra guias procedentes para além do prazo de 18 dias (42,84% fora do prazo em set/2026).
* **Achado Crítico 2 — Risco Regulatório Oculto na Média:** Embora a média global de pagamento (24 dias) esteja abaixo dos 30 dias, a dispersão é severa. Atrasos de 126 a 187 dias (SE e RR) e picos sazonais de mais de 3.300 guias atrasadas em abril representam risco direto de abertura de NIPs e penalização no IDSS.

### 5.2 Revisão de Lacunas e Decisão de Iteração

Conforme a anatomia iterativa do CRISP-DM, recuar para enriquecer os dados não representa falha, mas aumento de precisão. A avaliação identificou uma lacuna estrutural relevante: os painéis atuais mostram **ONDE** (UF) e **QUANDO** ocorre o reembolso, mas ainda não discriminam o **PROCEDIMENTO/ESPECIALIDADE** (terminologia TUSS) nem o **MOTIVO PADRONIZADO DA NÃO APROVAÇÃO**.

**Decisão Executiva:**
1. **APROVAR** a implementação imediata dos painéis atuais para controle diário de SLA e gestão financeira;
2. **INICIAR** a Iteração 2 (retornando às Fases 2 e 3) para integrar à base os códigos TUSS, especialidades médicas e motivos de indeferimento.

---

## Fase 6: Implementação

> **Objetivo Central da Fase 6 (CRISP-DM):** Incorporar formalmente os painéis e descobertas à rotina decisória da operadora, estabelecendo plano de ação por área, governança de monitoramento contínuo e registro das lições aprendidas.

### 6.1 Plano de Ação Estratégico por Setor

| Área de Negócio | Ação Estratégica Recomendada | Impacto Esperado |
| :--- | :--- | :--- |
| **Credenciamento** | Priorizar expansão e negociação de rede nas UFs com maior volume de reembolsos e picos de 2026 (especialmente SE, RR e PR), atacando a causa raiz da RN 566/22. | Redução estrutural da medida reparadora e proteção do IDSS. |
| **Sinistro e Regulação** | Revisar fluxos de auditoria nos estados com estouro do SLA de 18 dias (PR, RR, TO, AC, AP, BA) e criar força-tarefa para liquidar o passivo acima de 30 dias. | Queda nas guias fora do prazo e mitigação de NIPs na ANS. |
| **Qualidade e Comunicação** | Implementar checklists obrigatórios e validação documental prévia na Internet e no Aplicativo (72,14% das entradas) para orientar o beneficiário antes do protocolo. | Redução expressiva da taxa de 50,79% de não aprovações e ganho de capacidade operacional. |
| **Compliance e Jurídico** | Auditar mensalmente os *outliers* acima de 60 dias (ex.: casos de 126 a 187 dias) e monitorar a curva de cumprimento da RN 566/22 junto à ANS. | Prevenção de multas, sanções administrativas e intervenções. |

### 6.2 Plano de Monitoramento Contínuo e Governança

* **Cadência de Acompanhamento:** Reunião mensal de governança de reembolso após o fechamento contábil do mês anterior, garantindo que análises de tendência desconsiderem meses em processamento parcial (como out/nov 2026).
* **Painel de Metas (KPIs de Controle):**
  1. Custo Assistencial Total $\le 85\%$;
  2. Aderência ao SLA de Análise ($\le 18$ dias) $\ge 90\%$;
  3. Aderência ao SLA de Pagamento ($\le 30$ dias) $\ge 95\%$;
  4. Taxa de Indeferimento por Erro Documental em queda contínua.
* **Manutenção contra Obsolescência:** Revisão trimestral das regras de negócio no Power BI e atualização automática do modelo após a incorporação dos dados de procedimentos TUSS e motivos de negativa.

### 6.3 Revisão do Projeto e Lições Aprendidas

A aplicação estruturada do CRISP-DM demonstrou que o problema do reembolso assistencial não é apenas financeiro, mas eminentemente operacional e regulatório. Duas lições centrais se destacam para a gestão:

1. **Indicadores médios mascaram riscos graves:** a média de 24 dias de pagamento oculta riscos regulatórios severos concentrados em estados específicos;
2. **Gargalo invisível da não aprovação:** a alta taxa de solicitações não aprovadas (50,79%) sobrecarrega a auditoria e atrasa guias legítimas.

Com a implantação destes painéis e o avanço planejado para a Iteração 2 (enriquecimento com TUSS e modelagem preditiva), a operadora estabelece uma base sólida para reconduzir o custo assistencial à faixa saudável de até 85% e fortalecer seu desempenho no IDSS.