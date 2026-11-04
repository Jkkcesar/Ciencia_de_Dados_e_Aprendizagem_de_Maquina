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
| Título provisório do projeto | Reembolso em Dados: onde agir para reduzir o reembolso, fortalecer o uso da rede credenciada e diminuir o custo assistencial |
| Curso / disciplina | Sistemas de Informação |
| Turma | Ciência de Dados e Aprendizagem de Máquina |
| Equipe | Júlio César de Aguiar Nunes/ Eduardo Salgueiro Evangelista  |
| Integrantes e funções iniciais | Júlio César: engenharia de dados (SQL/ETL), governança e LGPD.Eduardo Salgueiro Evangelista: análise, modelagem e visualização. |
| Professor(a) | Gabriel |
| Data de elaboração | 16-09-2026 |
| Versão do documento | V04 |

## 2. Visão geral

### 2.1 Resumo do projeto

A operadora, uma **autogestão em saúde**, usará **os próprios dados de reembolso assistencial** para descobrir **onde agir**: em quais **estados e municípios**, em quais **procedimentos e especialidades** e com quais **prestadores** o reembolso é mais utilizado. A hipótese de trabalho é que parte desse reembolso decorre de **lacunas ou de baixa utilização da rede credenciada**.

Com esse diagnóstico, a gestão poderá priorizar ações como credenciar ou reforçar a rede onde há demanda reembolsada, orientar beneficiários sobre a rede disponível e rever fluxos. O resultado esperado é **reduzir o reembolso, tornar constante o uso da rede credenciada e diminuir o custo assistencial** associado à **medida reparadora** prevista na **RN nº 566/2022 da ANS**, que trata da garantia de atendimento e do reembolso quando o atendimento não é garantido na rede.

O projeto **não busca negar nem dificultar reembolsos devidos**. Busca eliminar as causas que levam o beneficiário a precisar deles. Os dados são tratados de forma **minimizada e pseudonimizada**, com saídas agregadas e decisão sempre humana.

### 2.2 Declaração do projeto em uma frase

> Nosso projeto utilizará **[dados ou fonte]** para compreender/prever **[fenômeno]**, apoiando **[público ou organização]** na decisão de **[decisão ou ação]**.

**Versão da equipe:**
> Nosso projeto utilizará **os dados internos de reembolso assistencial e de rede credenciada da própria operadora** para compreender **onde (UF e município), em quais procedimentos, especialidades e prestadores o reembolso se concentra**, apoiando **a gestão da autogestão em saúde** na decisão de **ampliar e direcionar a rede credenciada e as ações de orientação, reduzindo o reembolso e o custo assistencial da medida reparadora (RN 566/2022 da ANS)**.

## 3. Contexto e definição do problema

### 3.1 Contexto

- **Onde o problema ocorre?** No processo de atendimento e reembolso da operadora, na fronteira entre a rede credenciada e o atendimento fora dela, em territórios (UF e município), procedimentos, especialidades e prestadores específicos.
- **Quem é afetado?** A operadora, pelo custo assistencial; os beneficiários, que precisam pagar e solicitar reembolso, esperar decisão e, às vezes, buscar atendimento longe; e a rede credenciada, subutilizada em alguns locais.
- **Quais sinais, dados ou relatos indicam sua existência?** A base registra, por item pago, o valor, o tipo e a forma de reembolso, a especialidade, o procedimento, a UF e a cidade de atendimento, o prestador executante e o motivo da decisão. A operadora identifica reembolsos recorrentes em certos locais e serviços, mas não tem uma leitura sistemática de **onde a rede falha** ou **onde é subutilizada**.
- **Por que é importante investigá-lo agora?** O reembolso como medida reparadora tem custo mais alto e menos previsível do que o atendimento na rede, além de gerar carga operacional. A RN 566/2022 reforça a obrigação de garantir atendimento, e os dados necessários já estão estruturados (`stg_prata` e `prata`).

> **Nota regulatória:** a descrição da RN 566/2022 deve ser confirmada com as áreas de regulação e jurídica da operadora, inclusive quanto às situações em que o reembolso é obrigatório (medida reparadora) e às em que decorre de livre escolha prevista em contrato.

### 3.2 Problema central

> **Modelo:** [Público/organização] enfrenta [problema observável] no contexto de [situação], produzindo [consequência ou impacto].

**Problema definido:**

> **A operadora de autogestão** enfrenta **reembolsos recorrentes concentrados em determinados municípios, estados, procedimentos, especialidades e prestadores, sem diagnóstico sistemático de quanto disso decorre de lacunas ou subutilização da rede credenciada** no contexto **da garantia de atendimento e da medida reparadora prevista na RN 566/2022 da ANS**, produzindo **custo assistencial maior e menos previsível, carga operacional e experiência pior para o beneficiário**.

### 3.3 Evidências iniciais

| Evidência | Fonte | O que ela indica? | Confiabilidade / limitação |
| :--- | :--- | :--- | :--- |
| **1.** A base permite situar cada item reembolsado por UF, cidade, especialidade, procedimento e prestador. | `stg_prata.fato_reembolso_itens_pagos` (`UF`, `UFATENDIMENTO`, `CIDADEUFATENDIMENTO`, `NROSERVICOPAGO`, `DESSERVICOPAGO`, `NMECONTRATADOEXECUTANTE`). | É possível identificar concentrações de reembolso por território, serviço e prestador. | **Limitação:** a cidade é a do atendimento, que pode diferir da residência do beneficiário. |
| **2.** A base traz tipo e forma de reembolso e motivo da decisão. | `SkTipoReembolso`, `NMEFORMAREEMBOLSO`, `dim_tipo_reembolso`, `dim_motivo_decisao_reembolso`. | Pode distinguir o reembolso por medida reparadora de outras formas, se as dimensões permitirem. | **A validar** com regulação: o mapeamento entre tipo/motivo e a RN 566/2022. |
| **3.** A base registra valores solicitados e pagos por guia e por item. | `VLRSOLICITADOGUIA`, `VLRTOTALPAGOGUIA`, `VLRPAGOITEM`, `QTDSERVICO`. | Permite dimensionar o custo do reembolso e, comparado ao custo na rede, o ganho potencial. | **Limitação:** valores de guia se repetem entre itens. O custo na rede depende de outra base (seção 8). |
| **4.** Prazo de finalização e indicador de 30 dias. | `PRAZOFINALIZACAO`, `FINALIZADOPRAZO30DIAS`. | Mede a carga e a experiência do processo de reembolso. | `DTAFINALIZACAO` tem valores-padrão inválidos, tratados como nulos. |

## 4. Público-alvo e partes interessadas

### 4.1 Público-alvo principal

| Aspecto | Descrição |
| :--- | :--- |
| **Quem são os usuários ou beneficiários?** | Diretoria e gerências da operadora; áreas de rede/credenciamento, reembolso, regulação, controle de custos e financeiro/atuária. |
| **Quais necessidades possuem?** | Saber onde a rede precisa ser ampliada ou mais utilizada, quais serviços e prestadores concentram o reembolso e quanto custo pode ser evitado. |
| **Como são afetados pelo problema?** | Custo maior e menos previsível, volume de análise de reembolsos e risco regulatório por falhas na garantia de atendimento. |
| **Que decisão ou ação poderão tomar com os resultados?** | Priorizar credenciamento e negociação por município, UF, especialidade e procedimento; orientar beneficiários e a rede; revisar fluxos de encaminhamento; e acompanhar o efeito das ações sobre o reembolso e o custo. |

### 4.2 Partes interessadas

| Parte interessada | Interesse no projeto | Influência | Forma de envolvimento |
| :--- | :--- | :--- | :--- |
| Diretoria / gestão | Reduzir custo assistencial e risco regulatório | Alta | Patrocínio e decisão |
| Rede credenciada / credenciamento | Direcionar expansão e relacionamento com prestadores | Alta | Validação do diagnóstico e execução das ações |
| Área de reembolso | Reduzir volume e retrabalho | Alta | Validação das regras e do dicionário |
| Regulação / jurídico | Conformidade com a RN 566/2022 e demais normas da ANS | Alta | Interpretação da norma e validação do enquadramento |
| Financeiro / atuária / controle de custos | Medir custo e economia potencial | Alta | Dados de custo na rede e validação dos cálculos |
| DPO, TI e segurança | Conformidade com a LGPD | Alta | Aprovação da base e do acesso |
| Equipe de BI | Consumo dos indicadores | Média | Integração com o Power BI |
| Orientação acadêmica | Rigor metodológico | Média | Orientação e avaliação |
| Beneficiários e prestadores (indireto) | Acesso à rede e processo justo | Indireta | Beneficiados pelas melhorias, sem exposição individual |

## 5. Objetivos do projeto

### 5.1 Objetivo geral

Identificar, a partir dos dados internos de reembolso e de rede credenciada, **onde a operadora pode agir** (estados, municípios, procedimentos, especialidades e prestadores) para **reduzir o reembolso, aumentar o uso constante da rede credenciada e diminuir o custo assistencial da medida reparadora** prevista na RN 566/2022 da ANS, apoiando a decisão da gestão em conformidade com a LGPD.

### 5.2 Objetivos específicos

| Nº | Objetivo específico | Evidência de conclusão |
|---:| :--- | :--- |
| **1** | Construir uma base analítica tratada e pseudonimizada, com dicionário de dados e classificação LGPD, e classificar o reembolso (medida reparadora, livre escolha, outros). | Script SQL versionado, regra de classificação validada pela regulação e relatório de qualidade aprovado pelo DPO. |
| **2** | Mapear onde o reembolso se concentra: por UF, município, especialidade, procedimento e prestador (curva ABC e Pareto). | Painel e relatório com ranking agregado e supressão de pequenos grupos. |
| **3** | Estimar a **lacuna de rede**: comparar reembolso com a oferta e a utilização da rede credenciada por território e especialidade. | Indicador de lacuna por município × especialidade, validado com a área de rede. |
| **4** | Estimar o **custo evitável**: diferença entre o custo do reembolso e o custo do mesmo serviço na rede. | Estimativa por segmento, com premissas documentadas e validadas com o financeiro. |
| **5** | Priorizar ações (credenciamento, negociação, orientação e fluxo) por impacto e viabilidade. | Portfólio priorizado (matriz impacto × esforço) aprovado pela gestão. |
| **6** | Definir indicadores de acompanhamento e, se couber, modelos de apoio (previsão de demanda de reembolso e anomalias). | Painel de acompanhamento e *model card*, com desempenho superior a uma linha de base. |

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
| **1** | Em quais estados e municípios o reembolso mais se concentra, em volume e em valor? | Priorizar territórios para ação na rede. | `UF`, `UFATENDIMENTO`, `CIDADEUFATENDIMENTO`, `VLRPAGOITEM`, datas. | Ranking, mapa e tendência por território. |
| **2** | Quais procedimentos e especialidades concentram o reembolso em cada território? | Definir quais especialidades e serviços credenciar ou reforçar. | Serviço, `especialidade`, `NMETPOITEM`, valores. | Curva ABC e Pareto por território. |
| **3** | Quais prestadores mais recebem reembolso e em que contexto (território, especialidade)? | Avaliar credenciamento ou negociação com prestadores recorrentes. | Prestador (pseudonimizado), território, especialidade, valores. | Ranking de prestadores e concentração (por exemplo, índice de Herfindahl). |
| **4** | Quanto do reembolso ocorre onde já existe rede credenciada para o mesmo serviço, e quanto ocorre onde não existe? | Distinguir problema de **acesso** (lacuna) de problema de **utilização** (orientação e fluxo). | Reembolso e cadastro da rede (município, especialidade). | Indicador de lacuna e de subutilização. |
| **5** | Qual é a parcela do reembolso classificada como medida reparadora e qual é o seu custo? | Dimensionar o impacto da RN 566/2022. | Tipo e forma de reembolso, motivo, valores. | Participação e custo por tipo. |
| **6** | Quanto o custo assistencial poderia diminuir se parte desse reembolso fosse atendida na rede? | Calcular o retorno das ações e priorizar. | Valor reembolsado e custo médio na rede por serviço. | Custo evitável estimado e cenários. |
| **7** | Que ações têm melhor relação entre impacto e esforço? | Compor o portfólio de iniciativas. | Resultados das perguntas 1 a 6. | Matriz impacto × esforço. |
| **8** | Depois das ações, o reembolso diminuiu e o uso da rede aumentou de forma sustentada? | Acompanhamento e ajuste. | Séries mensais de reembolso e de uso da rede. | Série temporal e comparação antes e depois por território. |

## 7. Hipóteses iniciais

Hipóteses são ponto de partida, e as análises podem gerar novas.

| Hipótese | Como poderá ser testada? | Resultado que a refutaria? |
|---|---|---|
| H1. Poucos municípios e especialidades concentram a maior parte do valor reembolsado. | Curva ABC e Pareto por território e especialidade. | Distribuição uniforme, sem concentração útil para priorização. |
| H2. Municípios com pouca ou nenhuma rede credenciada na especialidade têm mais reembolso por beneficiário (efeito da lacuna). | Correlação e regressão entre oferta de rede e reembolso, por município × especialidade. | Ausência de relação entre oferta e reembolso. |
| H3. Parte relevante do reembolso ocorre onde **há** rede credenciada, o que indica subutilização ou falha de encaminhamento. | Cruzamento de reembolso com a existência de rede para o mesmo serviço e território. | Quase todo o reembolso ocorre onde não há rede. |
| H4. Poucos prestadores concentram o reembolso e são candidatos a credenciamento ou negociação. | Ranking e índice de concentração por especialidade e território. | Reembolso pulverizado entre muitos prestadores. |
| H5. O custo do reembolso por serviço é maior do que o custo médio na rede. | Comparação por serviço entre o valor reembolsado e o custo médio na rede. | Custos equivalentes ou menores no reembolso. |
| H6. Prazo e canal de solicitação variam entre territórios e acentuam a carga do reembolso. | Comparação de prazo e canal por território. | Sem diferença entre territórios. |

## 8. Dados necessários e viabilidade

| Conjunto ou fonte de dados | Variáveis principais | Formato | Acesso / responsável | Qualidade esperada |
|---|---|---|---|---|
| `stg_prata.fato_reembolso_itens_pagos` | Datas, prazos, situação da guia, valores, tipo e serviço do item, quantidade, UF e cidade, prestador, chaves das dimensões. | Tabela relacional (SQL Trino/Presto) | Interno e restrito / área de dados e DPO | Alta, com ressalvas (datas inválidas, repetição por guia, nulos). |
| `stg_prata.dim_canal_solicitacao`, `dim_especialidade_atendimento`, `dim_motivo_decisao_reembolso`, `dim_tipo_reembolso`, `prata.dim_conveniada`, `prata.dim_plano` | Descrições categóricas. | Tabelas relacionais | Interno / área de dados | Alta, com possível falta de correspondência. |
| **Cadastro da rede credenciada** (a obter) | Prestador (tipo PJ/PF), município, UF, especialidade, serviços, situação do credenciamento. | Tabela relacional / planilha | Interno / área de rede | A avaliar. **É essencial** para medir lacuna e subutilização. |
| **Utilização e custo na rede credenciada** (a obter) | Volume e custo médio por serviço, território e período (contas médicas, sem identificação de beneficiário). | Tabela agregada | Interno / financeiro e atuária | A avaliar. **É essencial** para custo evitável e para a relação rede × reembolso. |
| **Base de beneficiários agregada** (a obter) | Contagem de beneficiários por município de residência, plano e faixa etária. | Tabela agregada | Interno / cadastro | A avaliar. Permite normalizar por beneficiário. |
| Dados públicos de apoio (opcional) | IBGE: população e região por município. | CSV / API | Público | Média/Alta |

### 8.1 Avaliação inicial dos dados

- **Disponibilidade:** a base de reembolso existe e é acessível. As bases de rede, custo na rede e beneficiários agregados precisam ser solicitadas, e sem elas só é possível descrever o reembolso (perguntas 1, 2, 3, 5 e 8 em parte), não medir lacuna e custo evitável (4 e 6).
- **Granularidade:** **item pago**. Valores da guia se repetem entre itens, e a análise no nível da guia exige agregação prévia.
- **Território:** a cidade e a UF são as do **atendimento**. Para o acesso, o ideal é também o município de **residência** do beneficiário, em nível agregado.
- **Volume e período:** a definir (sugestão: 24 a 36 meses), com comparação antes e depois das ações.
- **Dados ausentes, duplicados ou inconsistentes:**
  - `DTAFINALIZACAO` vem como texto. Os valores `''`, `0`, `00000000`, `19000101` e `1900-01-01` viram nulos, os formatos `AAAAMMDD` e `AAAA-MM-DD[ hh:mm:ss]` são convertidos e os demais viram nulos.
  - Nomes de cidade e de prestador podem ter grafias diferentes. É preciso padronizar (por exemplo, pelo código IBGE) e deduplicar prestadores.
  - Descrições nulas após o `LEFT JOIN` viram "Não informado".
  - Guias sem finalização são analisadas à parte.
- **Integração entre fontes:** reembolso e rede por município (código IBGE), especialidade e serviço, com tabela de equivalência de procedimentos. Chaves `Sk*` para as dimensões.
- **Restrições legais, contratuais ou institucionais:** a base contém dados pessoais e **dados pessoais sensíveis de saúde** (LGPD, arts. 5º, II, 11 e 12). Como controladora, a operadora precisa de finalidade e base legal documentadas e parecer do DPO. Também devem ser respeitadas as normas da ANS, inclusive a RN 566/2022, e eventuais cláusulas contratuais com a rede.

**Classificação LGPD e tratamento das colunas**

| Coluna(s) | Classificação | Tratamento |
|---|---|---|
| `NMEBENEFICIARIO` | Identificador direto | **Excluir** da base de análise e do painel. |
| `NroCIBPaciente` | Identificador direto | **Pseudonimizar** (HMAC com chave em cofre). Usar só como chave de agrupamento. |
| `NROGSP`, `NROGSPVINCULADA` | Identificador indireto | **Pseudonimizar**. `TIPOGSPVINCULADA` pode ser mantido. |
| `NROCPFCNPJCONTRATADOEXECUTANTE`, `NMECONTRATADOEXECUTANTE` | Identificador de prestador (PJ ou PF) | **Pseudonimizar** o documento e **excluir** o nome na base de modelagem. Manter o tipo de prestador (PJ/PF). Ver regra de reidentificação autorizada abaixo. |
| `NroInsConselho`, `CodCBO` | Identificador profissional | `NroInsConselho` pseudonimizado. `CodCBO` mantido de forma agregada. |
| `NroPrtAnestesico`, `NmeTermo` | Identificador indireto / possível texto livre | **Excluir** o primeiro e revisar o segundo. |
| `CID` | **Dado sensível de saúde** | Usar apenas o grupo (3 caracteres), com **supressão** de categorias raras (< 11 casos) e de CIDs de maior estigma em saídas. |
| `DESSERVICOPAGO`, `NMETPOITEM`, `NROSERVICOPAGO` | Dado de saúde (procedimento) | Usar no nível de procedimento ou de grupo, com supressão de pequenos grupos. |
| `DTASOLICITACAOGUIA`, `DTAFINALIZACAO`, `DTA_ATENDIMENTO` | Identificador indireto | Usar nos cálculos e **generalizar para mês/ano** nas saídas. |
| `UF`, `UFATENDIMENTO`, `CIDADEUFATENDIMENTO` | Identificador indireto | Manter UF. Município em saídas somente com grupo mínimo (< 11 casos vão para "Outros do estado"). |
| `VLR*`, `QTDSERVICO`, `PRAZOFINALIZACAO`, `FINALIZADOPRAZO30DIAS`, `SITUACAOGUIA` | Não identificável isoladamente | Manter, sem expor valores individuais com atributos identificadores. |
| `Sk*`, `canal_solicitacao`, `especialidade`, `motivo_decisao`, `conveniada`, `plano`, `tipo_reembolso` | Atributos categóricos | Manter. `especialidade` e `motivo_decisao` podem revelar saúde e têm o mesmo cuidado do `CID`. |

**Prestadores (pessoa jurídica × pessoa física):** a análise por prestador (pergunta 3) é central, mas o CPF e o nome de uma pessoa física são dados pessoais. Regra proposta: a análise é feita com o prestador pseudonimizado. A **identificação para fins de credenciamento ou negociação** é feita pela área de rede, em ambiente restrito, por solicitação registrada ao DPO/TI, e **só para prestadores PJ ou para PF com finalidade e base legal documentadas**. Os rankings divulgados fora da área de rede mostram apenas tokens, nunca nomes.

### 8.2 Privacidade, ética e segurança

- [ ] A equipe verificou se há dados pessoais ou sensíveis (sim: saúde e identificadores).
- [ ] A coleta e o uso dos dados possuem finalidade legítima e explícita (gestão da rede e do custo assistencial) e base legal definida pelo DPO.
- [ ] O acesso será limitado às pessoas autorizadas.
- [ ] Dados pessoais serão minimizados, anonimizados ou pseudonimizados quando necessário.
- [ ] Possíveis vieses e impactos sobre grupos serão analisados.
- [ ] A divulgação dos resultados evitará reidentificação ou exposição indevida.

**Cuidados específicos deste projeto:**

1. **Dados ficam na operadora:** análise em ambiente interno e controlado, sem envio a serviços externos ou ferramentas de IA de terceiros.
2. **Minimização na origem:** a extração para análise não seleciona nomes, CPF/CNPJ, números de guia, carteira, conselho ou protocolo anestésico em claro. A pseudonimização (HMAC-SHA256) ocorre na própria consulta, e a chave fica com o DPO/TI.
3. **Separação de ambientes:** a tabela de correspondência (token ↔ original) fica fora do ambiente de análise e só é consultada nos termos da regra de prestadores acima.
4. **Anonimização nas saídas:** supressão de células com menos de 11 registros, generalização de datas e controle de cruzamentos (CID × município × data × valor). Municípios pequenos são agrupados.
5. **Nível agregado, não individual:** os resultados são por território, serviço, especialidade e prestador. Não há modelo ou ranking de beneficiários.
6. **Direito ao reembolso preservado:** as ações visam ampliar e orientar a rede. É vedado usar os resultados para **dificultar, atrasar ou negar** reembolsos devidos, e isso deve constar do termo de uso do painel.
7. **Uso restrito:** os resultados não podem servir a seleção de risco, precificação individual ou restrição de cobertura.
8. **Prestadores:** os rankings têm finalidade de gestão da rede e não de penalização. Devem passar por revisão da regulação e do jurídico antes de qualquer ação de credenciamento ou descredenciamento.
9. **Revisão humana:** os modelos apoiam, e a decisão é da gestão (LGPD, art. 20).
10. **Governança:** ROPA, RIPD, prazo de retenção, registro de acessos e plano de incidentes. No Power BI, RLS e somente visões agregadas.
11. **Repositório e documento:** sem dados reais, *prints* com informações pessoais ou credenciais.

## 9. Escopo do projeto

| Dentro do escopo | Fora do escopo |
|---|---|
| 1. Extração, tratamento e pseudonimização dos dados internos de reembolso. | 1. Decisão automatizada de deferimento ou indeferimento de reembolso. |
| 2. Diagnóstico por UF, município, especialidade, procedimento e prestador. | 2. Identificação ou monitoramento de beneficiários individuais. |
| 3. Indicadores de lacuna de rede, subutilização e custo evitável (se as bases de rede e de custo forem obtidas). | 3. Redução de reembolso por meio de barreiras, atrasos ou negativas. |
| 4. Priorização de ações (credenciamento, negociação, orientação e fluxo) e painel de acompanhamento. | 4. Execução do credenciamento, de contratos ou de negociações. |
| 5. Modelos de apoio (previsão de demanda de reembolso e anomalias) e *model card*. | 5. Interpretação jurídica definitiva da RN 566/2022 e implantação em produção. |
| 6. Documentação de governança (dicionário, classificação LGPD, RIPD). | 6. Publicação de dados brutos ou de amostras reidentificáveis. |

**Restrições conhecidas:**

- Prazo acadêmico definido pelo período letivo e equipe de uma pessoa.
- Acesso dependente de autorização formal do DPO e da gestão.
- Dependência das bases de rede, de custo na rede e de beneficiários agregados.
- Qualidade dos dados (datas inválidas, repetição por guia, grafia de cidade e prestador).
- Privacidade: pseudonimização obrigatória e supressão de pequenos grupos.
- Necessidade de validar regras de negócio e a norma com regulação e jurídico.

## 10. Resultados e entregáveis previstos

| Entregável | Descrição | Formato | Responsável | Critério de aceite |
|---|---|---|---|---|
| Base tratada | Consulta SQL versionada, com tratamento de datas, padronização de município e prestador, enriquecimento e pseudonimização. | Script SQL e tabela analítica | Júlio César | Sem identificadores diretos, chaves validadas e aprovação do DPO. |
| Dicionário de dados e classificação LGPD | Descrição e tratamento de cada coluna e regra de classificação do reembolso. | Documento / planilha | Júlio César | 100% das colunas classificadas. |
| Diagnóstico territorial e de serviços | Concentração do reembolso por UF, município, especialidade, procedimento e prestador. | Relatório / notebook | Júlio César | Perguntas 1 a 3 e 5 respondidas com agregados seguros. |
| Análise de lacuna de rede e custo evitável | Cruzamento com a rede e o custo na rede, com premissas documentadas. | Relatório / planilha | Júlio César | Perguntas 4 e 6 respondidas e validadas com rede e financeiro. |
| Portfólio de ações priorizado | Iniciativas por território, especialidade e prestador, com impacto estimado, esforço e indicador. | Documento / matriz | Júlio César | Aprovado pela gestão. |
| Painel de acompanhamento | Indicadores de reembolso, uso da rede e custo, por território, com comparação antes e depois. | Power BI | Júlio César | Sem dados pessoais, com RLS e validado com a gestão. |
| Modelos de apoio | Previsão de demanda de reembolso e anomalias por segmento. | Notebooks e artefatos | Júlio César | Superar a linha de base em teste temporal. |
| Relatório final | Método, resultados, limitações, *model card* e riscos éticos. | PDF / apresentação | Júlio César | Aprovação do professor e da gestão. |

## 11. Critérios de sucesso

| Critério | Indicador ou evidência | Meta | Forma de verificação |
|---|---|---|---|
| Relevância para o problema | Perguntas respondidas e aceitas pela gestão. | Perguntas 1 a 7 respondidas e validadas. | Reunião de validação e ata. |
| Ações acionáveis | Iniciativas priorizadas com território, especialidade, responsável e indicador. | Ao menos 5 iniciativas aprovadas pela gestão. | Revisão com rede, reembolso e financeiro. |
| Efeito esperado (pós-projeto) | Redução do reembolso evitável, aumento do uso da rede e queda do custo assistencial da medida reparadora nos territórios-alvo. | Metas definidas com a gestão (por exemplo, queda percentual do reembolso evitável em 12 meses). | Série mensal antes e depois, com grupo de comparação. |
| Qualidade dos dados | Nulos, duplicidades, datas inválidas, padronização de município e prestador e integridade das chaves. | Datas inválidas tratadas em 100% dos casos. Correspondência das chaves acima do acordado (sugestão: 98%). | Relatório de perfilamento. |
| Qualidade dos modelos | Desempenho em teste temporal. | Ganho mínimo sobre a linha de base, definido após a exploração. | Validação temporal e análise de erro por segmento. |
| Conformidade com a LGPD | Identificadores nos artefatos e células pequenas. | 0 identificadores diretos e 0 células com menos de 11 registros. | Auditoria do DPO e varredura automática. |
| Conformidade com o uso | Ausência de uso dos resultados para dificultar reembolsos devidos. | 100% das ações revisadas pela regulação. | Registro de revisão. |

## 12. Plano inicial de trabalho

| Etapa | Atividades principais | Responsável(is) | Prazo | Dependências |
|---|---|---|---|---|
| 1. Definição | Alinhar problema, escopo, perguntas e hipóteses. Validar o enquadramento da RN 566/2022 e a base legal. | Júlio César | Semanas 1 e 2 | Nenhuma |
| 2. Obtenção dos dados | Adaptar a consulta (minimização e pseudonimização). Solicitar cadastro da rede, custo na rede e beneficiários agregados. | Júlio César | Semanas 3 e 4 | Etapa 1, DPO e áreas donas dos dados |
| 3. Preparação dos dados | Perfilamento, tratamento de nulos e datas, padronização de município (IBGE) e prestador, classificação do reembolso. | Júlio César | Semanas 5 e 6 | Etapa 2 |
| 4. Diagnóstico | Concentração por território, serviço, especialidade e prestador. Lacuna de rede e custo evitável. | Júlio César | Semanas 7 e 8 | Etapa 3 |
| 5. Priorização e modelos | Portfólio de ações (impacto × esforço) e modelos de apoio. | Júlio César | Semanas 9 e 10 | Etapa 4 |
| 6. Validação | Revisão com rede, reembolso, financeiro e regulação. Validação temporal, viés e auditoria de privacidade. | Júlio César | Semana 11 | Etapa 5 |
| 7. Comunicação | Painel, *model card*, relatório e apresentação. | Júlio César | Semana 12 | Etapa 6 |

## 13. Riscos do projeto

| Risco | Probabilidade | Impacto | Estratégia de resposta | Responsável |
|---|---|---|---|---|
| Reidentificação ou vazamento de dados de saúde e de prestadores pessoa física. | Média | Alto | Minimização, pseudonimização na origem, supressão, regra de reidentificação autorizada e RIPD. | Júlio César / DPO |
| Bases de rede, custo na rede e beneficiários indisponíveis ou de baixa qualidade. | Alta | Alto | Solicitar cedo, usar proxies (por exemplo, serviços e prestadores com reembolso recorrente) e declarar a limitação. | Júlio César / Rede |
| Interpretação incorreta da RN 566/2022 ou da classificação do reembolso. | Média | Alto | Validar com regulação e jurídico antes de apresentar valores. | Júlio César / Regulação |
| Atribuir a lacuna de rede um reembolso que decorre de livre escolha ou de outro motivo. | Média | Médio | Classificar o reembolso por tipo e motivo e apresentar o resultado por categoria. | Júlio César |
| Uso dos resultados para dificultar reembolsos devidos ou para penalizar prestadores. | Baixa | Alto | Termo de uso, revisão da regulação e foco em ações sobre a rede. | Gestão / Regulação |
| Qualidade dos dados (datas inválidas, grafia de município e prestador, guias em aberto). | Alta | Médio | Regras documentadas, padronização por código IBGE e deduplicação de prestadores. | Júlio César |
| Dupla contagem por causa da granularidade de item. | Média | Alto | Agregar por guia antes de somar e reconciliar. | Júlio César |
| Correlação interpretada como causa (reembolso × rede). | Média | Médio | Tratar achados como hipóteses, validar com as áreas e medir o efeito antes e depois. | Júlio César |
| Equipe de uma pessoa e prazo curto. | Média | Médio | Priorizar o diagnóstico e as ações e deixar os modelos como etapa opcional. | Júlio César |

## 14. Organização da equipe

| Integrante | Papel principal | Responsabilidades | Apoio necessário |
|---|---|---|---|
| Júlio César de Aguiar Nunes | Engenharia de dados, governança, análise e modelagem | Consulta SQL, pseudonimização, dicionário, qualidade, LGPD, diagnóstico, priorização, modelos, painel e documentação. | DPO, TI, áreas de rede, reembolso, financeiro e regulação, e orientação. |

## 15. Validação da definição do projeto

Antes da entrega, confirme:

- [ ] O problema é real, relevante e delimitado.
- [ ] O público-alvo e as partes interessadas estão identificados.
- [ ] O objetivo geral e os objetivos específicos são coerentes.
- [ ] As perguntas de negócio orientam decisões concretas.
- [ ] Há dados disponíveis para responder às perguntas (inclusive rede e custo na rede).
- [ ] O escopo é compatível com o prazo e os recursos.
- [ ] Os critérios de sucesso são mensuráveis.
- [ ] Riscos, privacidade, ética e segurança foram considerados.
- [ ] O enquadramento da RN 566/2022 foi validado pela regulação.
- [ ] Nenhum dado pessoal real consta neste documento ou nos anexos.

## 16. Aprovação e registro de ajustes

| Responsável | Validação / observação | Data |
|---|---|---|
| Representante da equipe (Júlio César) | | |
| Professor(a) / orientador(a) (Gabriel) | | |
| Encarregado de dados (DPO) | Parecer sobre base legal, minimização e pseudonimização. | |
| Regulação / jurídico | Validação do enquadramento da RN 566/2022. | |

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

1. Remover do `SELECT` final `NMEBENEFICIARIO` e, na base de modelagem, `NMECONTRATADOEXECUTANTE`.
2. Tokenizar (HMAC com chave em cofre) `NROGSP`, `NROGSPVINCULADA`, `NroCIBPaciente`, `NROCPFCNPJCONTRATADOEXECUTANTE` e `NroInsConselho`.
3. Remover `NroPrtAnestesico` e revisar `NmeTermo`.
4. Reduzir `CID` ao grupo (3 caracteres) e generalizar datas para mês/ano nas saídas agregadas.
5. Calcular `Guias` e `Valor_pagos` antes da tokenização, para não quebrar as medidas do Power BI.
6. Não somar `VLRSOLICITADOGUIA` e `VLRTOTALPAGOGUIA` no nível de item, pois se repetem entre itens da mesma guia.

**Ajustes adicionais para o objetivo de rede e custo (V04):**

7. Padronizar município e UF de atendimento (preferencialmente por código IBGE) e deduplicar prestadores antes de qualquer ranking.
8. Derivar a classificação do reembolso (medida reparadora, livre escolha e outros) a partir de `SkTipoReembolso`, `NMEFORMAREEMBOLSO` e `motivo_decisao`, conforme regra validada pela regulação.
9. Integrar, por município e especialidade, o cadastro da rede credenciada e o custo na rede, em nível agregado e sem dados de beneficiários.
