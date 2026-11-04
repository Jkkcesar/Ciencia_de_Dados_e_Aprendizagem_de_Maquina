# Template - Definição do Projeto de Ciência de Dados

**Unidade:** III - Gestão de Projetos  
**Metodologia:** PBL + trabalho em equipes  
**Entregável:** Documento de definição do projeto

> **Finalidade:** delimitar um problema real e orientar o desenvolvimento do projeto de Ciência de Dados. Preencha todos os campos com informações objetivas, verificáveis e coerentes entre si.
>
> **Aviso LGPD:** este documento descreve apenas **estrutura, metadados e regras de tratamento** dos dados. Ele **não contém** nomes, CPF/CNPJ, números de guia, carteirinha, CID por paciente, registros de conselho ou quaisquer outros dados pessoais ou de saúde reais. Valores de exemplo são fictícios.

## 1. Identificação do projeto

| Campo | Preenchimento |
|---|---|
| Título provisório do projeto | Reembolso Inteligente: aprendizagem de máquina para apoio à gestão do reembolso assistencial |
| Curso / disciplina | Sistemas de Informação |
| Turma | Ciência de Dados e Aprendizagem de Máquina |
| Equipe | Júlio César de Aguiar Nunes, Eduardo Salgueiro Evangelista |
| Integrantes e funções iniciais | Júlio César: engenharia de dados (SQL/ETL), governança e LGPD. Eduardo: modelagem de aprendizagem de máquina, avaliação e visualização. |
| Professor(a) | Gabriel |
| Data de elaboração | 16-09-2026 |
| Versão do documento | V02 |

## 2. Visão geral

### 2.1 Resumo do projeto

O projeto aplica ciência de dados e aprendizagem de máquina ao **reembolso assistencial** de uma **operadora de saúde de autogestão**. A base de origem é a tabela fato de **itens pagos de reembolso**, enriquecida por dimensões descritivas (canal de solicitação, especialidade, motivo de decisão, conveniada, plano e tipo de reembolso), extraída por consulta SQL (Trino/Presto) com tratamento da data de finalização e granularidade de **item pago**.

Os objetivos são: (i) entender o comportamento do reembolso (volume, valores, prazos e motivos); (ii) **prever o prazo de finalização** e o risco de descumprimento do limite de 30 dias; (iii) **detectar valores e padrões atípicos** que mereçam auditoria; e (iv) gerar indicadores agregados para a gestão e para pesquisa. Todo o trabalho é feito com dados **pseudonimizados e minimizados**, sem exposição de beneficiários ou prestadores individuais.

**Preenchimento:** a equipe deve revisar este resumo a cada nova versão da extração.

### 2.2 Declaração do projeto em uma frase

> Nosso projeto utilizará **[dados ou fonte]** para compreender/prever **[fenômeno]**, apoiando **[público ou organização]** na decisão de **[decisão ou ação]**.

**Versão da equipe:**
> Nosso projeto utilizará **dados pseudonimizados de itens pagos de reembolso assistencial** para compreender e prever **o prazo de finalização das solicitações, o descumprimento do limite de 30 dias e valores atípicos**, apoiando **a gestão e as áreas técnicas e de auditoria de uma autogestão em saúde** na decisão de **priorizar a análise de solicitações, ajustar fluxos e canais, revisar regras de reembolso e direcionar auditorias**.

## 3. Contexto e definição do problema

### 3.1 Contexto

- **Onde o problema ocorre?** No processo de reembolso assistencial de uma operadora de saúde de autogestão: da solicitação da guia (canal de solicitação) até a decisão, a finalização e o pagamento dos itens.
- **Quem é afetado?** Beneficiários, que dependem de decisões rápidas e previsíveis. Também são afetados as áreas operacionais de reembolso, a auditoria e a gestão da operadora, responsável pelo equilíbrio financeiro da carteira.
- **Quais sinais, dados ou relatos indicam sua existência?** A base já registra, por guia e por item, `PRAZOFINALIZACAO` e `FINALIZADOPRAZO30DIAS`, o que permite medir o desempenho do prazo. Também registra valores solicitados e pagos (`VLRSOLICITADOGUIA`, `VLRTOTALPAGOGUIA`, `VLRPAGOITEM`), a situação da guia e o motivo da decisão. Indicadores iniciais (a confirmar na análise exploratória): proporção de guias fora do prazo, diferença entre valor solicitado e valor pago, concentração de valores por especialidade, tipo de reembolso e conveniada.
- **Por que é importante investigá-lo agora?** O reembolso é uma despesa de difícil previsão. Atrasos afetam a satisfação do beneficiário e o cumprimento de prazos, e glosas e divergências consomem tempo da equipe. A base está estruturada em camadas (`stg_prata` e `prata`), o que permite construir modelos com governança.

### 3.2 Problema central

> **Modelo:** [Público/organização] enfrenta [problema observável] no contexto de [situação], produzindo [consequência ou impacto].

**Problema definido:**

> **A gestão de uma operadora de saúde de autogestão** enfrenta **dificuldade de antecipar o prazo de finalização, de identificar fatores de atraso e de localizar valores atípicos entre os itens pagos de reembolso** no contexto **do processo de reembolso assistencial, com alto volume de itens e múltiplos canais, especialidades, planos e tipos de reembolso**, produzindo **atrasos acima de 30 dias, retrabalho, risco financeiro e decisões pouco baseadas em evidências**.

### 3.3 Evidências iniciais

| Evidência | Fonte | O que ela indica? | Confiabilidade / limitação |
| :--- | :--- | :--- | :--- |
| **1.** A base registra prazo de finalização e indicador de finalização em até 30 dias por guia. | Tabela `stg_prata.fato_reembolso_itens_pagos` (campos `PRAZOFINALIZACAO`, `FINALIZADOPRAZO30DIAS`). | Permite medir o descumprimento de prazo e usá-lo como alvo de aprendizagem supervisionada. | **Confiabilidade a validar** com a área dona do dado. **Limitação:** `DTAFINALIZACAO` possui valores padrão e inválidos (nulo, `0`, `19000101`) tratados como nulos, o que pode ocultar guias ainda em aberto. |
| **2.** A base registra valor solicitado da guia, valor total pago e valor pago por item. | Mesma tabela fato (`VLRSOLICITADOGUIA`, `VLRTOTALPAGOGUIA`, `VLRPAGOITEM`). | Permite estudar glosa, divergência de valores e valores atípicos por tipo de serviço. | **Limitação:** a granularidade é de item pago, e valores de guia se repetem entre itens. É preciso evitar dupla contagem ao somar. |
| **3.** Dimensões descritivas permitem segmentar a análise. | `dim_canal_solicitacao`, `dim_especialidade_atendimento`, `dim_motivo_decisao_reembolso`, `dim_conveniada`, `dim_plano`, `dim_tipo_reembolso`. | Possibilita comparar prazo, valor e motivos de decisão por segmento. | **Limitação:** `LEFT JOIN` preserva a fato, mas chaves sem correspondência geram categorias nulas, que exigem tratamento ("Não informado"). |

## 4. Público-alvo e partes interessadas

### 4.1 Público-alvo principal

| Aspecto | Descrição |
| :--- | :--- |
| **Quem são os usuários ou beneficiários?** | Gestores e coordenadores da operadora, áreas de reembolso e de regulação, auditoria e controle de custos, planejamento e pesquisadores internos. Os beneficiários são o público final beneficiado, mas **não são usuários nem alvo de exposição individual**. |
| **Quais necessidades possuem?** | Monitorar prazos e custos, entender causas de atraso e de divergência, priorizar filas e auditorias, e obter indicadores confiáveis e agregados para decisão e pesquisa. |
| **Como são afetados pelo problema?** | Atrasos e decisões inconsistentes geram insatisfação e retrabalho. A falta de previsibilidade dificulta o planejamento financeiro. |
| **Que decisão ou ação poderão tomar com os resultados?** | Priorizar solicitações com risco de atraso, ajustar fluxos e canais, reforçar a orientação sobre documentação, revisar regras por tipo de reembolso e especialidade, e direcionar amostras de auditoria. |

### 4.2 Partes interessadas

| Parte interessada | Interesse no projeto | Influência | Forma de envolvimento |
| :--- | :--- | :--- | :--- |
| Diretoria / gestão da autogestão | Eficiência, sustentabilidade financeira e conformidade com prazos | Alta | Patrocínio e consumo dos indicadores |
| Área de reembolso (operação) | Reduzir atrasos e retrabalho | Alta | Validação das regras de negócio e do dicionário de dados |
| Auditoria e controle de custos | Identificar valores atípicos e inconsistências | Média/Alta | Validação das alertas e retorno sobre os casos analisados |
| Encarregado de dados (DPO) / TI e segurança | Conformidade com a LGPD e segurança da informação | Alta | Aprovação da base pseudonimizada e do controle de acesso |
| Equipe de BI (Power BI) | Consumo da consulta e dos indicadores | Média | Integração e manutenção do modelo semântico |
| Corpo docente / orientação | Rigor metodológico | Média | Orientação e avaliação |
| Beneficiários (indireto) | Decisões rápidas, justas e com privacidade | Indireta | Beneficiados pelas melhorias, sem acesso aos dados individuais |

## 5. Objetivos do projeto

### 5.1 Objetivo geral

Desenvolver uma solução de ciência de dados e aprendizagem de máquina que, a partir de dados pseudonimizados de itens pagos de reembolso assistencial, preveja o prazo de finalização, o risco de descumprimento do limite de 30 dias e padrões atípicos de valor, apoiando a gestão de uma autogestão em saúde em decisões operacionais, financeiras e de pesquisa, em conformidade com a LGPD.

### 5.2 Objetivos específicos

| Nº | Objetivo específico | Evidência de conclusão |
|---:| :--- | :--- |
| **1** | Construir uma base analítica tratada e pseudonimizada, a partir da consulta de extração, com dicionário de dados e classificação LGPD por coluna. | Script SQL versionado, base sem identificadores diretos, dicionário de dados e relatório de qualidade aprovados pelo DPO. |
| **2** | Realizar análise exploratória de prazo, valor pago, glosa e motivos de decisão por canal, especialidade, plano, conveniada e tipo de reembolso. | Relatório analítico com indicadores agregados e regras de supressão de pequenos grupos. |
| **3** | Treinar e avaliar modelos supervisionados para prever o descumprimento do prazo de 30 dias (classificação) e o tempo de finalização (regressão). | Métricas em conjunto de teste temporal (por exemplo, AUC-PR, recall, F1 e MAE) superiores a uma linha de base simples. |
| **4** | Aplicar detecção de anomalias para sinalizar valores e padrões atípicos para auditoria. | Lista de segmentos ou itens pseudonimizados sinalizados, com taxa de confirmação mínima definida com a auditoria. |
| **5** | Disponibilizar os resultados em painel gerencial com explicabilidade e documentação de uso responsável. | Painel funcional, importância de variáveis (por exemplo, SHAP) e *model card* publicados. |

### 5.3 Verificação dos objetivos

Marque após revisar:

- [x] São específicos e escritos com clareza.
- [x] Podem ser verificados por meio de entregáveis ou métricas.
- [x] São viáveis com os dados, recursos e tempo disponíveis.
- [x] Estão diretamente relacionados ao problema central.
- [x] Consideram os usuários e a decisão que será apoiada.

## 6. Perguntas de negócio

| Nº | Pergunta de negócio | Decisão apoiada | Dados necessários | Análise ou indicador possível |
|---:| :--- | :--- | :--- | :--- |
| **1** | Quais fatores (canal, especialidade, tipo de reembolso, plano, conveniada, motivo de decisão) mais se associam ao tempo de finalização e ao descumprimento do prazo de 30 dias? | Ajustar fluxos, filas e capacitação por segmento. | `PRAZOFINALIZACAO`, `FINALIZADOPRAZO30DIAS`, `DTASOLICITACAOGUIA`, `DTAFINALIZACAO`, dimensões. | Percentual no prazo por segmento, modelo de classificação e importância de variáveis. |
| **2** | Quanto o valor pago difere do valor solicitado e quais motivos de decisão explicam essa diferença? | Revisar regras e orientar sobre documentação. | `VLRSOLICITADOGUIA`, `VLRTOTALPAGOGUIA`, `VLRPAGOITEM`, `motivo_decisao`. | Taxa de glosa, distribuição de diferenças e análise por motivo. |
| **3** | Quais itens ou segmentos apresentam valores atípicos em relação ao padrão de serviço e especialidade? | Direcionar auditoria amostral. | `VLRPAGOITEM`, `QTDSERVICO`, `NROSERVICOPAGO`, `DESSERVICOPAGO`, `especialidade`, `NMETPOITEM`. | Detecção de anomalias (Isolation Forest, escore robusto por grupo). |
| **4** | Como o volume e o valor de reembolso evoluem ao longo do tempo e por região (UF/cidade, em nível agregado)? | Planejamento financeiro e previsão de demanda. | Datas, `UF`, `UFATENDIMENTO`, `CIDADEUFATENDIMENTO`, valores. | Séries temporais, previsão e mapa por UF. |
| **5** | Quais tipos de serviço e especialidades concentram o maior gasto, e como isso se distribui por plano e conveniada? | Revisão de políticas de cobertura e negociação. | Item, especialidade, plano, conveniada, valores. | Curva ABC, Pareto e custo médio por serviço. |
| **6** | Quais perfis de solicitação demoram mais que o esperado, mesmo depois de controlar o segmento? | Priorização antecipada de guias em risco. | Variáveis da pergunta 1. | Escore de risco, com validação temporal e análise de erro por grupo. |

## 7. Hipóteses iniciais

| Hipótese | Como poderá ser testada? | Resultado que a refutaria? |
|---|---|---|
| H1. O canal de solicitação e o tipo de reembolso influenciam o prazo de finalização. | Comparar a distribuição do prazo entre canais e tipos (teste não paramétrico) e medir a importância no modelo. | Diferenças não significativas e importância irrelevante após controle por especialidade. |
| H2. Guias com maior valor solicitado e maior número de itens tendem a demorar mais. | Correlação de Spearman e regressão do prazo sobre o valor e o número de itens. | Associação ausente ou negligenciável. |
| H3. Há diferença sistemática entre valor solicitado e valor pago, explicada por motivo de decisão e especialidade. | Análise da glosa por motivo e especialidade. | Diferenças que se distribuem de forma aleatória entre os segmentos. |
| H4. Valores atípicos concentram-se em poucos serviços e especialidades. | Detecção de anomalias por grupo e verificação com a auditoria. | Anomalias dispersas sem relação com grupos e sem confirmação pela auditoria. |
| H5. Um modelo supervisionado supera uma regra simples (mediana por segmento) na previsão de atraso. | Validação temporal comparada à linha de base. | Desempenho igual ou inferior à linha de base. |

## 8. Dados necessários e viabilidade

| Conjunto ou fonte de dados | Variáveis principais | Formato | Acesso / responsável | Qualidade esperada |
|---|---|---|---|---|
| `stg_prata.fato_reembolso_itens_pagos` (fato de itens pagos) | Datas (solicitação, finalização, atendimento), prazos, situação da guia, valores, tipo e serviço do item, quantidade, UF e cidade de atendimento, chaves das dimensões. | Tabela relacional consultada por SQL (Trino/Presto) | Interno, restrito / área de dados e DPO | Alta, com ressalvas: datas-padrão inválidas, valores repetidos por guia e nulos. |
| Dimensões `stg_prata.dim_canal_solicitacao`, `dim_especialidade_atendimento`, `dim_motivo_decisao_reembolso`, `dim_tipo_reembolso` | Descrições categóricas. | Tabelas relacionais | Interno / área de dados | Alta, com possível falta de correspondência. |
| Dimensões `prata.dim_conveniada` e `prata.dim_plano` | Nome da conveniada e do plano. | Tabelas relacionais | Interno / área de dados | Alta |
| Dados públicos de apoio (opcional): IBGE (região, população) e tabelas de terminologia de procedimentos | Região, população, categorias de procedimentos. | CSV / API | Público | Média/Alta |

### 8.1 Avaliação inicial dos dados

- **Disponibilidade:** dados internos, acessíveis por consulta SQL, com acesso restrito e justificado por finalidade.
- **Granularidade:** **item pago**. Há várias linhas por guia, e `VLRSOLICITADOGUIA` e `VLRTOTALPAGOGUIA` se repetem entre as linhas da mesma guia. A guia só é um ponto de dado para o modelo de prazo quando as linhas forem agregadas, com uma linha por guia.
- **Volume e período coberto:** a definir na extração (preferencialmente 24 a 36 meses), com separação temporal entre treino, validação e teste.
- **Dados ausentes, duplicados ou inconsistentes previstos:**
  - `DTAFINALIZACAO` vem como texto, e os valores `''`, `0`, `00000000`, `19000101` e `1900-01-01` são tratados como nulos. Formatos `AAAAMMDD` e `AAAA-MM-DD[ hh:mm:ss]` são convertidos para data, e valores inválidos viram nulos.
  - Guias sem data de finalização podem estar em aberto (censura), e o modelo de prazo deve tratá-las separadamente.
  - `LEFT JOIN` gera descrições nulas, que serão padronizadas como "Não informado".
  - Duplicidades são eliminadas na consulta final por `DISTINCT`, preservando a granularidade do item.
- **Necessidade de integração entre fontes:** integração por chaves substitutas (`Sk*`) das dimensões. Enriquecimento externo apenas em nível agregado (UF ou região).
- **Restrições legais, contratuais ou institucionais:** a base contém **dados pessoais e dados pessoais sensíveis de saúde** (LGPD, arts. 5º, II, 11 e 12). O tratamento exige base legal e finalidade definidas, parecer do DPO e conformidade com as normas da ANS e da operadora. Deve ser considerada também a regulação da pesquisa em seres humanos, se houver publicação.

**Classificação LGPD e tratamento das colunas da consulta**

| Coluna(s) | Classificação | Tratamento no projeto |
|---|---|---|
| `NMEBENEFICIARIO` | Identificador direto | **Excluir** da base de modelagem e do painel. |
| `NroCIBPaciente` | Identificador direto (carteira/CIB do paciente) | **Pseudonimizar** com hash com sal (HMAC) mantido em cofre de segredos. Usar apenas como chave de agrupamento, nunca exibir. |
| `NROGSP`, `NROGSPVINCULADA` | Identificador indireto (número da guia) | **Pseudonimizar** com token irreversível. `TIPOGSPVINCULADA` pode ser mantido. |
| `NROCPFCNPJCONTRATADOEXECUTANTE`, `NMECONTRATADOEXECUTANTE` | Identificador de prestador (pode ser pessoa física) | **Pseudonimizar** o CPF/CNPJ. **Excluir** o nome. Usar apenas o tipo de prestador e a UF. |
| `NroInsConselho`, `CodCBO` | Identificador profissional / categoria | `NroInsConselho` deve ser **pseudonimizado**. `CodCBO` pode ser mantido de forma agregada. |
| `NroPrtAnestesico`, `NmeTermo` | Identificador indireto / texto livre potencial | **Excluir** `NroPrtAnestesico`. Revisar `NmeTermo` e remover se houver texto livre. |
| `CID` | **Dado sensível de saúde** | Usar apenas no **nível de capítulo ou grupo** (3 caracteres). **Suprimir** categorias raras (< 11 casos) e CIDs de maior estigma (HIV, saúde mental, etc.) em saídas públicas. |
| `DESSERVICOPAGO`, `NMETPOITEM`, `NROSERVICOPAGO` | Dado de saúde (procedimento) | Manter no **nível de grupo de procedimento**. Aplicar supressão de pequenos grupos nas saídas agregadas. |
| `DTASOLICITACAOGUIA`, `DTAFINALIZACAO`, `DTA_ATENDIMENTO` | Identificador indireto (datas) | Manter apenas para cálculo e **generalizar para mês/ano** nas saídas. |
| `UF`, `UFATENDIMENTO`, `CIDADEUFATENDIMENTO` | Identificador indireto (geográfico) | Manter **UF**. Cidade somente se o grupo tiver tamanho mínimo, caso contrário usar região. |
| `VLR*`, `QTDSERVICO`, `PRAZOFINALIZACAO`, `FINALIZADOPRAZO30DIAS`, `SITUACAOGUIA` | Dado não identificável isoladamente | Manter. Valores individuais não devem ser expostos junto com atributos identificadores. |
| `Sk*`, `canal_solicitacao`, `especialidade`, `motivo_decisao`, `conveniada`, `plano`, `tipo_reembolso` | Atributos categóricos | Manter. `especialidade` e `motivo_decisao` podem indicar o estado de saúde, e o cuidado é o mesmo do `CID`. |

### 8.2 Privacidade, ética e segurança

- [X] A equipe verificou se há dados pessoais ou sensíveis (sim: dados de saúde e identificadores).
- [X] A coleta e o uso dos dados possuem finalidade legítima e explícita (gestão, auditoria e pesquisa), com base legal definida pelo DPO (por exemplo, tutela da saúde, estudos por órgão de pesquisa, ou execução de contrato).
- [X] O acesso será limitado às pessoas autorizadas, com perfis, registro de acessos (log) e ambiente restrito.
- [X] Dados pessoais serão minimizados, anonimizados ou pseudonimizados quando necessário.
- [X] Possíveis vieses e impactos sobre grupos serão analisados.
- [X] A divulgação dos resultados evitará reidentificação ou exposição indevida.

**Cuidados específicos deste projeto:**

1. **Minimização na origem:** a consulta de extração para modelagem **não deve selecionar** nomes, CPF/CNPJ, número de guia, carteira, número de conselho ou protocolo anestésico em claro. A pseudonimização (HMAC-SHA256 com chave em cofre de segredos) deve ocorrer **na própria extração**, e a chave não é compartilhada com a equipe de modelagem.
2. **Separação de ambientes:** a tabela de correspondência (token ↔ identificador original) fica sob custódia exclusiva do DPO / TI, fora do ambiente de análise.
3. **Anonimização nas saídas:** agregados com supressão de células com menos de 11 registros (k-anonimidade mínima), generalização de datas e de geografia, e sem cruzamentos que permitam reidentificação (CID × cidade × data × valor).
4. **Dados sensíveis de saúde:** CID e especialidade só em nível agregado e nunca em saídas públicas. Os modelos não usam atributos protegidos como alvo de discriminação, e o resultado do modelo **não pode negar ou restringir direitos** do beneficiário de forma automática.
5. **Decisão com revisão humana:** o modelo **apoia** a priorização e a auditoria. Não decide reembolso sozinho (LGPD, art. 20 — direito à revisão de decisões automatizadas).
6. **Segurança:** criptografia em trânsito e em repouso, controle de acesso mínimo, ausência de cópias locais e de envio a serviços externos ou ferramentas de IA de terceiros, e descarte seguro dos dados temporários.
7. **Governança:** registro das operações de tratamento (ROPA), relatório de impacto à proteção de dados (RIPD/DPIA), prazo de retenção e plano de resposta a incidentes.
8. **Power BI:** aplicar segurança em nível de linha (RLS) quando houver detalhamento, ocultar colunas identificadoras e publicar apenas visões agregadas.
9. **Este repositório e o documento:** não devem conter dados reais, exemplos reais, *prints* com informações pessoais nem credenciais.

## 9. Escopo do projeto

| Dentro do escopo | Fora do escopo |
|---|---|
| 1. Extração, tratamento e pseudonimização dos dados de itens pagos de reembolso. | 1. Decisão automatizada de deferimento ou indeferimento de reembolso. |
| 2. Análise exploratória e indicadores de prazo, valor, glosa e motivos. | 2. Identificação, perfilamento ou monitoramento de beneficiários ou prestadores individuais. |
| 3. Modelos de previsão de prazo, de classificação de risco de atraso e de detecção de anomalias. | 3. Uso dos dados para precificação individual, seleção de risco ou restrição de cobertura. |
| 4. Painel gerencial agregado, *model card* e dicionário de dados com classificação LGPD. | 4. Implantação em produção e integração com sistemas transacionais (somente protótipo). |
| 5. Documentação de governança (RIPD, plano de anonimização). | 5. Publicação de dados brutos ou de amostras com possibilidade de reidentificação. |

**Restrições conhecidas:**

- **Prazo acadêmico:** cronograma delimitado pelo período letivo.
- **Acesso a dados:** depende de autorização formal do DPO e da gestão, e pode ser limitado a um ambiente seguro.
- **Qualidade dos dados:** datas-padrão inválidas, itens repetidos por guia e guias em aberto (censura).
- **Privacidade e LGPD:** pseudonimização obrigatória, supressão de pequenos grupos e ausência de dados pessoais nos artefatos.
- **Conhecimento técnico:** necessidade de validação das regras de negócio com especialistas da área de reembolso.

## 10. Resultados e entregáveis previstos

| Entregável | Descrição | Formato | Responsável | Critério de aceite |
|---|---|---|---|---|
| Base tratada | Consulta SQL versionada (CTEs de tratamento de data, enriquecimento por dimensões e resultado final), com pseudonimização e minimização. | Script SQL e tabela analítica | Júlio César | Sem identificadores diretos, chaves validadas, duplicidades tratadas e aprovação do DPO. |
| Dicionário de dados e classificação LGPD | Descrição de cada coluna, regra de tratamento e nível de sensibilidade. | Documento / planilha | Júlio César | 100% das colunas classificadas e revisadas. |
| Análise exploratória | Indicadores de prazo, valor, glosa, motivos e tendência, por segmento. | Relatório analítico (notebook) | Eduardo Salgueiro | Perguntas 1 a 5 respondidas com agregados que respeitam a supressão mínima. |
| Modelos de aprendizagem de máquina | Previsão de atraso (classificação), de tempo de finalização (regressão) e detecção de anomalias. | Notebooks e artefatos versionados | Eduardo Salgueiro | Superar a linha de base em validação temporal, com análise de erros e de viés. |
| Visualizações / painel | Painel gerencial com indicadores agregados e escore de risco por segmento. | Power BI | Eduardo Salgueiro / Júlio César | Sem dados pessoais, RLS aplicada e validação com a gestão. |
| Relatório ou apresentação | Documento técnico com método, resultados, limitações, *model card* e riscos éticos. | PDF / apresentação | Equipe | Aprovação do professor e conformidade com a estrutura acadêmica. |

## 11. Critérios de sucesso

| Critério | Indicador ou evidência | Meta | Forma de verificação |
|---|---|---|---|
| Relevância para o problema | Perguntas de negócio respondidas e aceitas pela área de reembolso. | Perguntas 1 a 5 respondidas e validadas pela área de reembolso. | Reunião de validação e ata. |
| Qualidade dos dados | Proporção de nulos, duplicidades e datas inválidas, e integridade das chaves. | Datas inválidas tratadas em 100% dos casos. Chaves das dimensões com correspondência acima do limite acordado (por exemplo, 98%). | Relatório automático de qualidade (perfilamento). |
| Qualidade da análise e dos modelos | Desempenho em conjunto de teste temporal comparado à linha de base. | Ganho mínimo sobre a linha de base, definido após a análise exploratória. | Validação temporal, intervalos de confiança e análise de erro por segmento. |
| Conformidade com a LGPD | Ausência de identificadores diretos nos artefatos e supressão de células pequenas. | 0 identificadores diretos. 0 células com menos de 11 registros em saídas. | Auditoria do DPO e varredura automática de colunas e padrões (CPF, CNPJ, nomes). |
| Utilidade para o público-alvo | Uso do painel e do escore pela gestão em decisões reais. | Pelo menos uma decisão ou ação operacional apoiada, a ser documentada. | Entrevista com a gestão. |
| Comunicação dos resultados | Clareza do relatório e do *model card*. | Aprovação do professor e da gestão. | Apresentação final. |

## 12. Plano inicial de trabalho

| Etapa | Atividades principais | Responsável(is) | Prazo | Dependências |
|---|---|---|---|---|
| 1. Definição | Alinhar o problema, o escopo, as perguntas, as hipóteses e a base legal com o DPO. | Júlio César / Eduardo Salgueiro | Semanas 1 e 2 | Nenhuma |
| 2. Obtenção dos dados | Ajustar a consulta para minimização e pseudonimização. Extrair a base e documentar o dicionário e a classificação LGPD. | Júlio César | Semanas 3 e 4 | Etapa 1 e aprovação do DPO |
| 3. Preparação dos dados | Perfilamento, tratamento de nulos, datas e duplicidades, engenharia de variáveis e agregação por guia. | Júlio César / Eduardo Salgueiro | Semanas 5 e 6 | Etapa 2 |
| 4. Análise / modelagem | Análise exploratória, linha de base, modelos supervisionados, detecção de anomalias e explicabilidade. | Eduardo Salgueiro | Semanas 7 a 9 | Etapa 3 |
| 5. Validação | Validação temporal, análise de viés e de erros, revisão com a área de reembolso e auditoria de privacidade. | Júlio César / Eduardo Salgueiro | Semanas 10 e 11 | Etapa 4 |
| 6. Comunicação | Painel, *model card*, relatório final e apresentação. | Equipe | Semana 12 | Etapa 5 |

## 13. Riscos do projeto

| Risco | Probabilidade | Impacto | Estratégia de resposta | Responsável |
|---|---|---|---|---|
| Vazamento ou reidentificação de beneficiários e prestadores (violação da LGPD). | Média | Alto | Minimização e pseudonimização na origem, supressão de pequenos grupos, controle de acesso, RIPD e ausência de dados em repositórios. | Júlio César / DPO |
| Acesso aos dados negado ou atrasado. | Média | Alto | Iniciar a solicitação cedo, com plano B de dados sintéticos mantendo o dicionário. | Equipe |
| Qualidade dos dados (datas inválidas, duplicidades, guias em aberto). | Alta | Médio | Regras de tratamento documentadas, perfilamento e separação das guias em aberto. | Júlio César |
| Dupla contagem de valores por causa da granularidade de item. | Média | Alto | Agregação explícita por guia antes de somar valores e testes de reconciliação. | Júlio César |
| Vazamento de informação no modelo (usar variáveis conhecidas só após a finalização). | Média | Alto | Seleção de variáveis disponíveis no momento da previsão e validação temporal. | Eduardo Salgueiro |
| Viés ou uso indevido do modelo (discriminação, decisão automatizada). | Baixa/Média | Alto | Análise de equidade por segmento, uso apenas como apoio e revisão humana. | Eduardo Salgueiro / Gestão |
| Mudança nas regras de reembolso (*concept drift*). | Média | Médio | Monitoramento do desempenho e retreino periódico. | Eduardo Salgueiro |
| Prazo insuficiente. | Média | Médio | Priorizar o mínimo viável (exploração, modelo de atraso e painel). | Equipe |

## 14. Organização da equipe

| Integrante | Papel principal | Responsabilidades | Apoio necessário |
|---|---|---|---|
| Júlio César de Aguiar Nunes | Engenharia de dados e governança | Consulta SQL, pseudonimização, dicionário de dados, qualidade, conformidade com a LGPD e documentação. | DPO, TI e orientação do professor. |
| Eduardo Salgueiro Evangelista | Ciência de dados e modelagem | Análise exploratória, modelagem, avaliação, explicabilidade e painel. | Especialistas da área de reembolso e orientação do professor. |

## 15. Validação da definição do projeto

Antes da entrega, confirme:

- [ ] O problema é real, relevante e delimitado.
- [ ] O público-alvo e as partes interessadas estão identificados.
- [ ] O objetivo geral e os objetivos específicos são coerentes.
- [ ] As perguntas de negócio orientam decisões concretas.
- [ ] Há dados potencialmente disponíveis para responder às perguntas.
- [ ] O escopo é compatível com o prazo e os recursos.
- [ ] Os critérios de sucesso são mensuráveis.
- [ ] Riscos, privacidade, ética e segurança foram considerados.
- [ ] Funções e responsabilidades foram distribuídas.
- [ ] Nenhum dado pessoal real consta neste documento ou nos anexos.

## 16. Aprovação e registro de ajustes

| Responsável | Validação / observação | Data |
|---|---|---|
| Representante da equipe (Júlio César / Eduardo Salgueiro) | | |
| Professor(a) / orientador(a) (Gabriel) | | |
| Encarregado de dados (DPO) | Parecer sobre base legal, minimização e pseudonimização. | |

### Ajustes solicitados após a apresentação inicial

| Data | Ajuste solicitado | Responsável | Situação |
|---|---|---|---|
| | | | |

## Anexo A. Referência técnica da consulta de extração

A consulta atual usa três CTEs e um resultado final. Para uso em modelagem, ela deve ser adaptada conforme a seção 8.1.

| Etapa | Função |
|---|---|
| `BaseTratamento` | Lê `stg_prata.fato_reembolso_itens_pagos` e converte `DTAFINALIZACAO` para texto uma única vez (`TRIM(CAST(... AS VARCHAR))`). |
| `BaseDataTratada` | Converte o texto para `DATE`. Nulos, `''`, `0`, `00000000`, `19000101` e `1900-01-01` viram nulos. `AAAAMMDD` usa `DATE_PARSE` com `TRY`. `AAAA-MM-DD` (com ou sem hora) usa `TRY_CAST` dos 10 primeiros caracteres. Outros formatos viram nulos. |
| `BaseReembolso` | `LEFT JOIN` com as dimensões de canal, especialidade, motivo, conveniada, plano e tipo de reembolso, preservando os itens sem correspondência. |
| Resultado final | `SELECT DISTINCT` no nível de item pago. `Guias` vale 1 se `NROGSP` não for nulo, e 0 caso contrário. `Valor_pagos` é igual a `VLRPAGOITEM`. `Data_finalizacao` repete `DTAFINALIZACAO` em formato de data (alias mantido para o Power Query). |

**Ajustes obrigatórios para a versão de modelagem (LGPD):**

1. Remover do `SELECT` final `NMEBENEFICIARIO` e `NMECONTRATADOEXECUTANTE`.
2. Substituir por tokens (HMAC com chave em cofre) `NROGSP`, `NROGSPVINCULADA`, `NroCIBPaciente`, `NROCPFCNPJCONTRATADOEXECUTANTE` e `NroInsConselho`.
3. Remover `NroPrtAnestesico` e revisar `NmeTermo`.
4. Reduzir `CID` ao grupo (3 caracteres) e generalizar datas para mês/ano nas saídas agregadas.
5. Preservar `Guias` e `Valor_pagos` calculados antes da tokenização, para não quebrar as medidas do Power BI.
6. Evitar somar `VLRSOLICITADOGUIA` e `VLRTOTALPAGOGUIA` em nível de item, porque se repetem entre os itens da mesma guia.
