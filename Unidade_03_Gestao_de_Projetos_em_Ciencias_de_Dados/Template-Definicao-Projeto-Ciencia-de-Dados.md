# Template - Definição do Projeto de Ciência de Dados

**Unidade:** III - Gestão de Projetos  
**Metodologia:** PBL + trabalho em equipes  
**Entregável:** Documento de definição do projeto

> **Finalidade:** delimitar um problema real e orientar o desenvolvimento do projeto de Ciência de Dados. Preencha todos os campos com informações objetivas, verificáveis e coerentes entre si.
>
> **Aviso LGPD:** este documento descreve apenas **estrutura, metadados e regras de tratamento** dos dados. Não contém nomes, CPF/CNPJ, números de guia ou de carteira, CID por paciente, registros de conselho nem qualquer outro dado pessoal ou de saúde real. Exemplos são fictícios.

## 1. Identificação do projeto

| Campo | Preenchimento |
|---|---|
| Título provisório do projeto | Reembolso em Dados: a operadora estuda os próprios dados para gerar ideias e apoiar decisões |
| Curso / disciplina | Sistemas de Informação |
| Turma | Ciência de Dados e Aprendizagem de Máquina |
| Equipe | Júlio César de Aguiar Nunes |
| Integrantes e funções iniciais | Júlio César: engenharia de dados (SQL/ETL), governança e LGPD, análise, modelagem e visualização. |
| Professor(a) | Gabriel |
| Data de elaboração | 16-09-2026 |
| Versão do documento | V03 |

## 2. Visão geral

### 2.1 Resumo do projeto

Uma operadora de saúde de **autogestão** acumula, em sua própria base de **reembolso assistencial**, informação suficiente para se auditar e aprender sobre si mesma. O projeto propõe que a **operadora estude os próprios dados** (tabela fato de itens pagos e dimensões de canal, especialidade, motivo de decisão, conveniada, plano e tipo de reembolso) para **gerar ideias, hipóteses e evidências** que subsidiem a tomada de decisão da gestão.

A abordagem é **exploratória e orientada à descoberta**: descrever o que acontece, encontrar padrões e segmentos, formular hipóteses e só então testar modelos preditivos (prazo de finalização, risco de atraso acima de 30 dias, valores atípicos). Os resultados são **insumos para a decisão humana**, nunca decisões automáticas. Os dados permanecem dentro da operadora e são tratados de forma **minimizada e pseudonimizada**.

### 2.2 Declaração do projeto em uma frase

> Nosso projeto utilizará **[dados ou fonte]** para compreender/prever **[fenômeno]**, apoiando **[público ou organização]** na decisão de **[decisão ou ação]**.

**Versão da equipe:**
> Nosso projeto utilizará **os dados internos e pseudonimizados de reembolso assistencial da própria operadora** para compreender e prever **prazos, custos, divergências de valor e padrões atípicos**, apoiando **a gestão da autogestão em saúde** na decisão de **ajustar fluxos, regras e prioridades e de definir novas iniciativas com base em evidências**.

## 3. Contexto e definição do problema

### 3.1 Contexto

- **Onde o problema ocorre?** Dentro da operadora, no processo de reembolso assistencial, da solicitação da guia à decisão, finalização e pagamento.
- **Quem é afetado?** A gestão, que decide com pouca visibilidade; as áreas de reembolso e auditoria; e os beneficiários, que sofrem os efeitos de atrasos e inconsistências.
- **Quais sinais, dados ou relatos indicam sua existência?** A base já registra prazo e indicador de finalização em 30 dias, valores solicitados e pagos, situação da guia e motivo da decisão. Esses dados são usados hoje sobretudo para relatórios operacionais e pouco para descobrir padrões e gerar ideias. A extração já existe e alimenta o Power BI.
- **Por que é importante investigá-lo agora?** A operadora já possui os dados estruturados em camadas (`stg_prata` e `prata`), o que reduz o custo de começar. Tratar a própria base como fonte de conhecimento permite decidir com evidências, sem depender de dados externos nem compartilhar informação de saúde com terceiros.

### 3.2 Problema central

> **Modelo:** [Público/organização] enfrenta [problema observável] no contexto de [situação], produzindo [consequência ou impacto].

**Problema definido:**

> **A gestão da operadora de autogestão** enfrenta **o subaproveitamento dos próprios dados de reembolso, que dificulta identificar causas de atraso, divergências de valor e oportunidades de melhoria** no contexto **de um processo com alto volume de itens, canais, especialidades, planos e tipos de reembolso**, produzindo **decisões baseadas em percepção, retrabalho, atrasos acima de 30 dias e risco financeiro não monitorado**.

### 3.3 Evidências iniciais

| Evidência | Fonte | O que ela indica? | Confiabilidade / limitação |
| :--- | :--- | :--- | :--- |
| **1.** A base registra prazo de finalização e indicador de finalização em até 30 dias. | `stg_prata.fato_reembolso_itens_pagos` (`PRAZOFINALIZACAO`, `FINALIZADOPRAZO30DIAS`). | É possível medir desempenho de prazo e investigar suas causas. | **Confiabilidade a validar** com a área dona do dado. **Limitação:** `DTAFINALIZACAO` tem valores-padrão inválidos (nulo, `0`, `19000101`), tratados como nulos. |
| **2.** A base registra valor solicitado, valor total pago e valor pago por item. | Mesma tabela (`VLRSOLICITADOGUIA`, `VLRTOTALPAGOGUIA`, `VLRPAGOITEM`). | Permite estudar diferença entre solicitado e pago e custos por serviço. | **Limitação:** granularidade de item, com valores de guia repetidos entre itens. É preciso evitar dupla contagem. |
| **3.** Dimensões descritivas permitem segmentar a análise. | `dim_canal_solicitacao`, `dim_especialidade_atendimento`, `dim_motivo_decisao_reembolso`, `dim_conveniada`, `dim_plano`, `dim_tipo_reembolso`. | Possibilita comparar segmentos e descobrir onde estão os problemas. | **Limitação:** `LEFT JOIN` mantém itens sem correspondência, que viram categorias nulas ("Não informado"). |

## 4. Público-alvo e partes interessadas

### 4.1 Público-alvo principal

| Aspecto | Descrição |
| :--- | :--- |
| **Quem são os usuários ou beneficiários?** | Diretoria, gerências e coordenações da operadora, áreas de reembolso, regulação, auditoria, controle de custos e planejamento. |
| **Quais necessidades possuem?** | Entender o próprio processo, encontrar causas e padrões, priorizar ações e contar com indicadores confiáveis e agregados. |
| **Como são afetados pelo problema?** | Sem análise sistemática, as decisões dependem de relatórios fixos e da experiência individual, e oportunidades e riscos passam despercebidos. |
| **Que decisão ou ação poderão tomar com os resultados?** | Ajustar fluxos e canais, revisar regras por tipo de reembolso ou especialidade, orientar beneficiários sobre documentação, direcionar auditorias e definir novas iniciativas e pesquisas internas. |

### 4.2 Partes interessadas

| Parte interessada | Interesse no projeto | Influência | Forma de envolvimento |
| :--- | :--- | :--- | :--- |
| Diretoria / gestão | Decisões baseadas em evidência e sustentabilidade financeira | Alta | Patrocínio e consumo dos resultados |
| Área de reembolso | Reduzir atrasos e retrabalho | Alta | Validação das regras de negócio e das hipóteses |
| Auditoria e controle de custos | Identificar inconsistências e valores atípicos | Média/Alta | Validação dos achados |
| Encarregado de dados (DPO), TI e segurança | Conformidade com a LGPD | Alta | Aprovação da base e do controle de acesso |
| Equipe de BI | Consumo dos dados e indicadores | Média | Integração com o Power BI |
| Orientação acadêmica | Rigor metodológico | Média | Orientação e avaliação |
| Beneficiários (indireto) | Processo mais rápido e privacidade | Indireta | Beneficiados pelas melhorias, sem acesso a dados individuais |

## 5. Objetivos do projeto

### 5.1 Objetivo geral

Estruturar um ciclo de **autoanálise dos dados de reembolso assistencial pela própria operadora**, combinando análise exploratória, segmentação e modelos de aprendizagem de máquina, para **gerar ideias e hipóteses e subsidiar a tomada de decisão** da gestão, em conformidade com a LGPD.

### 5.2 Objetivos específicos

| Nº | Objetivo específico | Evidência de conclusão |
|---:| :--- | :--- |
| **1** | Construir uma base analítica tratada, minimizada e pseudonimizada a partir da consulta atual, com dicionário de dados e classificação LGPD. | Script SQL versionado, dicionário e relatório de qualidade aprovados pelo DPO. |
| **2** | Descrever o comportamento do reembolso (volume, valor, prazo, glosa e motivos) por segmento. | Relatório exploratório com indicadores agregados e supressão de pequenos grupos. |
| **3** | Descobrir padrões e segmentos relevantes (agrupamentos, associações, tendências) e transformá-los em hipóteses e ideias de ação. | Banco de ideias priorizado por impacto e viabilidade, validado com as áreas. |
| **4** | Testar modelos de apoio: previsão de atraso, de tempo de finalização e detecção de anomalias. | Métricas em teste temporal superiores a uma linha de base simples. |
| **5** | Entregar um painel e um roteiro de decisão para uso recorrente pela gestão. | Painel agregado, *model card* e ao menos uma decisão apoiada e documentada. |

### 5.3 Verificação dos objetivos

Marque após revisar:

- [ ] São específicos e escritos com clareza.
- [ ] Podem ser verificados por meio de entregáveis ou métricas.
- [ ] São viáveis com os dados, recursos e tempo disponíveis.
- [ ] Estão diretamente relacionados ao problema central.
- [ ] Consideram os usuários e a decisão que será apoiada.

## 6. Perguntas de negócio

| Nº | Pergunta de negócio | Decisão apoiada | Dados necessários | Análise ou indicador possível |
|---:| :--- | :--- | :--- | :--- |
| **1** | Onde e por que o reembolso demora mais de 30 dias? | Ajustar fluxos, filas e capacitação. | Prazos, datas, canal, especialidade, tipo, plano. | Percentual no prazo por segmento, árvore de decisão e importância de variáveis. |
| **2** | Quanto e por que o valor pago difere do solicitado? | Revisar regras e orientar documentação. | Valores da guia e do item, `motivo_decisao`. | Taxa de glosa e análise por motivo. |
| **3** | Que grupos naturais de solicitações existem e o que os distingue? | Criar abordagens específicas por perfil de solicitação. | Atributos agregados e categóricos (sem identificação). | Agrupamento (*clustering*) e descrição dos grupos. |
| **4** | Que itens ou segmentos apresentam valores atípicos? | Direcionar auditoria amostral. | `VLRPAGOITEM`, `QTDSERVICO`, serviço, especialidade. | Detecção de anomalias por grupo. |
| **5** | Como volume e custo evoluem no tempo e por UF? | Planejamento financeiro. | Datas, UF, valores. | Séries temporais e previsão. |
| **6** | Quais serviços e especialidades concentram o gasto? | Revisar políticas e negociação. | Item, especialidade, plano, conveniada, valores. | Curva ABC e Pareto. |
| **7** | Quais ideias de melhoria têm melhor relação entre impacto e esforço? | Priorizar o portfólio de iniciativas. | Resultados das perguntas 1 a 6. | Matriz impacto × esforço. |

## 7. Hipóteses iniciais

Hipóteses são ponto de partida, e as análises exploratórias podem gerar novas.

| Hipótese | Como poderá ser testada? | Resultado que a refutaria? |
|---|---|---|
| H1. Canal e tipo de reembolso influenciam o prazo. | Comparar prazos entre canais e tipos, com teste não paramétrico. | Diferenças não significativas após controle por especialidade. |
| H2. Guias com maior valor e mais itens demoram mais. | Correlação de Spearman e regressão. | Associação ausente. |
| H3. A diferença entre solicitado e pago concentra-se em poucos motivos e especialidades. | Análise de glosa por motivo e especialidade. | Distribuição uniforme entre segmentos. |
| H4. Existem perfis de solicitação com comportamento distinto de prazo e custo. | Agrupamento e comparação entre grupos. | Grupos instáveis ou sem diferença prática. |
| H5. Valores atípicos concentram-se em poucos serviços. | Detecção de anomalias e verificação com a auditoria. | Anomalias dispersas e não confirmadas. |
| H6. Um modelo supera uma regra simples (mediana por segmento) na previsão de atraso. | Validação temporal contra a linha de base. | Desempenho igual ou inferior. |

## 8. Dados necessários e viabilidade

| Conjunto ou fonte de dados | Variáveis principais | Formato | Acesso / responsável | Qualidade esperada |
|---|---|---|---|---|
| `stg_prata.fato_reembolso_itens_pagos` | Datas, prazos, situação da guia, valores, tipo e serviço do item, quantidade, UF e cidade, chaves das dimensões. | Tabela relacional (SQL Trino/Presto) | Interno e restrito / área de dados e DPO | Alta, com ressalvas (datas inválidas, repetição por guia, nulos). |
| `stg_prata.dim_canal_solicitacao`, `dim_especialidade_atendimento`, `dim_motivo_decisao_reembolso`, `dim_tipo_reembolso` | Descrições categóricas. | Tabelas relacionais | Interno / área de dados | Alta, com possível falta de correspondência. |
| `prata.dim_conveniada`, `prata.dim_plano` | Conveniada e plano. | Tabelas relacionais | Interno / área de dados | Alta |

O projeto usa **somente dados da própria operadora**. Dados externos (por exemplo, IBGE) são opcionais e entram apenas em nível agregado.

### 8.1 Avaliação inicial dos dados

- **Disponibilidade:** dados internos, acessíveis por SQL, com acesso restrito e justificado pela finalidade.
- **Granularidade:** **item pago**. Valores da guia se repetem entre os itens, e análises no nível da guia exigem agregação prévia.
- **Volume e período:** a definir na extração (sugestão: 24 a 36 meses), com separação temporal entre treino e teste.
- **Dados ausentes, duplicados ou inconsistentes:**
  - `DTAFINALIZACAO` vem como texto. Os valores `''`, `0`, `00000000`, `19000101` e `1900-01-01` viram nulos, os formatos `AAAAMMDD` e `AAAA-MM-DD[ hh:mm:ss]` são convertidos e os demais viram nulos.
  - Guias sem finalização podem estar em aberto e são analisadas à parte.
  - Descrições nulas após o `LEFT JOIN` viram "Não informado".
  - Duplicidades são removidas por `DISTINCT` no resultado final.
- **Integração entre fontes:** por chaves substitutas (`Sk*`) das dimensões.
- **Restrições legais, contratuais ou institucionais:** a base contém dados pessoais e **dados pessoais sensíveis de saúde** (LGPD, arts. 5º, II, 11 e 12). Como a operadora é a controladora, o uso interno para gestão e melhoria exige finalidade e base legal documentadas, parecer do DPO e respeito às normas da ANS. Participantes da equipe que não sejam da operadora só acessam a base pseudonimizada.

**Classificação LGPD e tratamento das colunas**

| Coluna(s) | Classificação | Tratamento |
|---|---|---|
| `NMEBENEFICIARIO` | Identificador direto | **Excluir** da base de análise e do painel. |
| `NroCIBPaciente` | Identificador direto | **Pseudonimizar** (HMAC com chave em cofre). Usar só como chave de agrupamento. |
| `NROGSP`, `NROGSPVINCULADA` | Identificador indireto | **Pseudonimizar**. `TIPOGSPVINCULADA` pode ser mantido. |
| `NROCPFCNPJCONTRATADOEXECUTANTE`, `NMECONTRATADOEXECUTANTE` | Identificador de prestador (pode ser pessoa física) | **Pseudonimizar** o documento e **excluir** o nome. |
| `NroInsConselho`, `CodCBO` | Identificador profissional | `NroInsConselho` pseudonimizado. `CodCBO` mantido de forma agregada. |
| `NroPrtAnestesico`, `NmeTermo` | Identificador indireto / possível texto livre | **Excluir** o primeiro e revisar o segundo. |
| `CID` | **Dado sensível de saúde** | Usar apenas o grupo (3 caracteres), com **supressão** de categorias raras (< 11 casos) e de CIDs de maior estigma em saídas. |
| `DESSERVICOPAGO`, `NMETPOITEM`, `NROSERVICOPAGO` | Dado de saúde (procedimento) | Usar no nível de grupo de procedimento e suprimir pequenos grupos. |
| `DTASOLICITACAOGUIA`, `DTAFINALIZACAO`, `DTA_ATENDIMENTO` | Identificador indireto | Usar nos cálculos e **generalizar para mês/ano** nas saídas. |
| `UF`, `UFATENDIMENTO`, `CIDADEUFATENDIMENTO` | Identificador indireto | Manter UF. Cidade apenas em grupos de tamanho mínimo. |
| `VLR*`, `QTDSERVICO`, `PRAZOFINALIZACAO`, `FINALIZADOPRAZO30DIAS`, `SITUACAOGUIA` | Não identificável isoladamente | Manter, sem expor valores individuais com atributos identificadores. |
| `Sk*`, `canal_solicitacao`, `especialidade`, `motivo_decisao`, `conveniada`, `plano`, `tipo_reembolso` | Atributos categóricos | Manter. `especialidade` e `motivo_decisao` podem revelar saúde e têm o mesmo cuidado do `CID`. |

### 8.2 Privacidade, ética e segurança

- [ ] A equipe verificou se há dados pessoais ou sensíveis (sim: saúde e identificadores).
- [ ] A coleta e o uso dos dados possuem finalidade legítima e explícita (gestão, auditoria e pesquisa interna) e base legal definida pelo DPO.
- [ ] O acesso será limitado às pessoas autorizadas.
- [ ] Dados pessoais serão minimizados, anonimizados ou pseudonimizados quando necessário.
- [ ] Possíveis vieses e impactos sobre grupos serão analisados.
- [ ] A divulgação dos resultados evitará reidentificação ou exposição indevida.

**Cuidados específicos deste projeto:**

1. **Dados ficam na operadora:** a análise ocorre em ambiente interno e controlado, sem envio a serviços externos ou ferramentas de IA de terceiros.
2. **Minimização na origem:** a extração para análise não seleciona nomes, CPF/CNPJ, números de guia, carteira, conselho ou protocolo anestésico em claro. A pseudonimização (HMAC-SHA256) ocorre na própria consulta, e a chave fica com o DPO/TI.
3. **Separação de ambientes:** a tabela de correspondência (token ↔ original) fica fora do ambiente de análise.
4. **Anonimização nas saídas:** supressão de células com menos de 11 registros, generalização de datas e geografia e controle de cruzamentos (CID × cidade × data × valor).
5. **Revisão humana:** os modelos geram ideias e priorizações, e não decidem sobre reembolso (LGPD, art. 20).
6. **Uso restrito:** os resultados não podem servir a seleção de risco, precificação individual ou restrição de cobertura.
7. **Governança:** ROPA, RIPD, prazo de retenção, registro de acessos e plano de incidentes.
8. **Power BI:** RLS quando houver detalhamento e publicação somente de visões agregadas.
9. **Repositório e documento:** sem dados reais, *prints* com informações pessoais ou credenciais.

## 9. Escopo do projeto

| Dentro do escopo | Fora do escopo |
|---|---|
| 1. Extração, tratamento e pseudonimização dos dados internos de reembolso. | 1. Decisão automatizada de deferimento ou indeferimento. |
| 2. Análise exploratória, segmentação e geração de hipóteses e ideias. | 2. Identificação ou monitoramento de beneficiários ou prestadores individuais. |
| 3. Modelos de apoio (atraso, tempo de finalização e anomalias). | 3. Precificação individual, seleção de risco ou restrição de cobertura. |
| 4. Painel agregado, banco de ideias priorizado e *model card*. | 4. Implantação em produção e integração com sistemas transacionais. |
| 5. Documentação de governança (dicionário, classificação LGPD, RIPD). | 5. Publicação de dados brutos ou de amostras reidentificáveis. |

**Restrições conhecidas:**

- Prazo acadêmico definido pelo período letivo.
- Acesso dependente de autorização formal do DPO e da gestão.
- Qualidade dos dados (datas inválidas, repetição por guia, guias em aberto).
- Privacidade: pseudonimização obrigatória e supressão de pequenos grupos.
- Necessidade de validar regras de negócio com especialistas de reembolso.

## 10. Resultados e entregáveis previstos

| Entregável | Descrição | Formato | Responsável | Critério de aceite |
|---|---|---|---|---|
| Base tratada | Consulta SQL versionada, com tratamento de datas, enriquecimento e pseudonimização. | Script SQL e tabela analítica | Júlio César | Sem identificadores diretos, chaves validadas e aprovação do DPO. |
| Dicionário de dados e classificação LGPD | Descrição e tratamento de cada coluna. | Documento / planilha | Júlio César | 100% das colunas classificadas. |
| Análise exploratória | Indicadores de prazo, valor, glosa, motivos e tendência. | Notebook / relatório | Júlio César | Perguntas 1 a 6 respondidas com agregados seguros. |
| Banco de ideias e hipóteses | Achados transformados em ideias, priorizadas por impacto e esforço. | Documento / matriz | Equipe | Validado com as áreas de reembolso e auditoria. |
| Modelos de apoio | Previsão de atraso, de tempo e anomalias. | Notebooks e artefatos | Júlio César | Superar a linha de base em teste temporal. |
| Painel | Indicadores agregados e escore de risco por segmento. | Power BI | Júlio César | Sem dados pessoais e com RLS. |
| Relatório final | Método, resultados, limitações, *model card* e riscos éticos. | PDF / apresentação | Equipe | Aprovação do professor e da gestão. |

## 11. Critérios de sucesso

| Critério | Indicador ou evidência | Meta | Forma de verificação |
|---|---|---|---|
| Relevância para o problema | Perguntas respondidas e aceitas pela gestão. | Perguntas 1 a 6 validadas. | Reunião de validação e ata. |
| Geração de ideias | Ideias priorizadas e validadas. | Ao menos 5 ideias com responsável e indicador de acompanhamento. | Revisão com as áreas. |
| Qualidade dos dados | Nulos, duplicidades, datas inválidas e integridade das chaves. | Datas inválidas tratadas em 100% dos casos. Correspondência das chaves acima do acordado (sugestão: 98%). | Relatório de perfilamento. |
| Qualidade dos modelos | Desempenho em teste temporal. | Ganho mínimo sobre a linha de base, definido após a exploração. | Validação temporal e análise de erro por segmento. |
| Conformidade com a LGPD | Identificadores nos artefatos e células pequenas. | 0 identificadores diretos e 0 células com menos de 11 registros. | Auditoria do DPO e varredura automática. |
| Utilidade para a decisão | Uso dos resultados pela gestão. | Ao menos uma decisão apoiada e documentada. | Entrevista com a gestão. |

## 12. Plano inicial de trabalho

| Etapa | Atividades principais | Responsável(is) | Prazo | Dependências |
|---|---|---|---|---|
| 1. Definição | Alinhar problema, escopo, perguntas, hipóteses e base legal com o DPO. | Júlio César | Semanas 1 e 2 | Nenhuma |
| 2. Obtenção dos dados | Adaptar a consulta (minimização e pseudonimização), extrair e documentar. | Júlio César | Semanas 3 e 4 | Etapa 1 e DPO |
| 3. Preparação dos dados | Perfilamento, tratamento de nulos e datas, variáveis e agregação por guia. | Júlio César | Semanas 5 e 6 | Etapa 2 |
| 4. Exploração e ideação | Análise exploratória, segmentação, hipóteses e banco de ideias. | Júlio César | Semanas 7 e 8 | Etapa 3 |
| 5. Modelagem | Linha de base, modelos, anomalias e explicabilidade. | Júlio César | Semanas 9 e 10 | Etapa 4 |
| 6. Validação | Validação temporal, viés, revisão com as áreas e auditoria de privacidade. | Júlio César | Semana 11 | Etapa 5 |
| 7. Comunicação | Painel, *model card*, relatório e apresentação. | Equipe | Semana 12 | Etapa 6 |

## 13. Riscos do projeto

| Risco | Probabilidade | Impacto | Estratégia de resposta | Responsável |
|---|---|---|---|---|
| Reidentificação ou vazamento de dados de saúde. | Média | Alto | Minimização, pseudonimização na origem, supressão, controle de acesso e RIPD. | Júlio César / DPO |
| Acesso aos dados negado ou atrasado. | Média | Alto | Solicitar cedo e ter dados sintéticos como plano B. | Equipe |
| Qualidade dos dados (datas inválidas, duplicidades, guias em aberto). | Alta | Médio | Regras documentadas e perfilamento. | Júlio César |
| Dupla contagem por causa da granularidade de item. | Média | Alto | Agregar por guia antes de somar e reconciliar. | Júlio César |
| Vazamento de informação no modelo (variáveis posteriores ao evento). | Média | Alto | Usar só variáveis disponíveis no momento da previsão. | Júlio César |
| Ideias sem aderência ao negócio ou sem adoção. | Média | Médio | Validar com as áreas e priorizar por impacto e esforço. | Equipe / Gestão |
| Viés ou uso indevido do modelo. | Baixa/Média | Alto | Análise por segmento, uso como apoio e revisão humana. | Júlio César / Gestão |
| Mudança de regras (*concept drift*). | Média | Médio | Monitorar e retreinar. | Júlio César |

## 14. Organização da equipe

| Integrante | Papel principal | Responsabilidades | Apoio necessário |
|---|---|---|---|
| Júlio César de Aguiar Nunes | Engenharia de dados, governança, análise e modelagem | Consulta SQL, pseudonimização, dicionário, qualidade, LGPD, exploração, hipóteses, modelos, explicabilidade, painel e documentação. | DPO, TI, especialistas de reembolso e orientação. |

## 15. Validação da definição do projeto

Antes da entrega, confirme:

- [ ] O problema é real, relevante e delimitado.
- [ ] O público-alvo e as partes interessadas estão identificados.
- [ ] O objetivo geral e os objetivos específicos são coerentes.
- [ ] As perguntas de negócio orientam decisões concretas.
- [ ] Há dados disponíveis para responder às perguntas.
- [ ] O escopo é compatível com o prazo e os recursos.
- [ ] Os critérios de sucesso são mensuráveis.
- [ ] Riscos, privacidade, ética e segurança foram considerados.
- [ ] Funções e responsabilidades foram distribuídas.
- [ ] Nenhum dado pessoal real consta neste documento ou nos anexos.

## 16. Aprovação e registro de ajustes

| Responsável | Validação / observação | Data |
|---|---|---|
| Representante da equipe (Júlio César) | | |
| Professor(a) / orientador(a) (Gabriel) | | |
| Encarregado de dados (DPO) | Parecer sobre base legal, minimização e pseudonimização. | |

### Ajustes solicitados após a apresentação inicial

| Data | Ajuste solicitado | Responsável | Situação |
|---|---|---|---|
| | | | |

## Anexo A. Referência técnica da consulta de extração

| Etapa | Função |
|---|---|
| `BaseTratamento` | Lê `stg_prata.fato_reembolso_itens_pagos` e converte `DTAFINALIZACAO` para texto uma única vez (`TRIM(CAST(... AS VARCHAR))`). |
| `BaseDataTratada` | Converte para `DATE`. Nulo, `''`, `0`, `00000000`, `19000101` e `1900-01-01` viram nulos. `AAAAMMDD` usa `DATE_PARSE` com `TRY`. `AAAA-MM-DD` (com ou sem hora) usa `TRY_CAST` dos 10 primeiros caracteres. Outros formatos viram nulos. |
| `BaseReembolso` | `LEFT JOIN` com as dimensões de canal, especialidade, motivo, conveniada, plano e tipo de reembolso. |
| Resultado final | `SELECT DISTINCT` no nível de item pago. `Guias` vale 1 se `NROGSP` não for nulo, senão 0. `Valor_pagos` é `VLRPAGOITEM`. `Data_finalizacao` repete `DTAFINALIZACAO` (alias mantido para o Power Query). |

**Ajustes obrigatórios para a versão de análise (LGPD):**

1. Remover do `SELECT` final `NMEBENEFICIARIO` e `NMECONTRATADOEXECUTANTE`.
2. Tokenizar (HMAC com chave em cofre) `NROGSP`, `NROGSPVINCULADA`, `NroCIBPaciente`, `NROCPFCNPJCONTRATADOEXECUTANTE` e `NroInsConselho`.
3. Remover `NroPrtAnestesico` e revisar `NmeTermo`.
4. Reduzir `CID` ao grupo (3 caracteres) e generalizar datas para mês/ano nas saídas agregadas.
5. Calcular `Guias` e `Valor_pagos` antes da tokenização, para não quebrar as medidas do Power BI.
6. Não somar `VLRSOLICITADOGUIA` e `VLRTOTALPAGOGUIA` no nível de item, pois se repetem entre itens da mesma guia.
