<img src="assets/logo-udf.jpg" alt="UDF Centro Universitário" width="230">

**Profa. Kadidja Valéria | UDF**  
Ciência de Dados e Aprendizado de Máquina · Sistemas de Informação · 2026.2

# Aula 1 — Conceitos e classificação de problemas

## Identificação

- Estudante ou equipe: **Júlio César de Aguiar Nunes**
- Turma: **Sistemas de Informação - 2026.2**
- Data de realização: **29/10/2026**
- Integrantes e contribuições (se em equipe): **Júlio César de Aguiar Nunes**

## Objetivos

Distinguir programação tradicional e Machine Learning, identificar atributos e alvo e diferenciar classificação, regressão e agrupamento.

## 1. Explicação com suas palavras

1. Como um modelo aprende com dados? **Em vez de ser programado com regras lógicas criadas por um humano, o modelo analisa um grande volume de dados durante o treinamento. Ele utiliza algoritmos para identificar padrões, correlações e regras estatísticas implícitas, ajustando seus parâmetros internos para fazer generalizações e tomar decisões sobre novos dados.**
2. Qual diferença entre algoritmo e modelo? **O algoritmo é o procedimento matemático ou lógico (a "receita" ou técnica, como Árvore de Decisão ou Regressão Linear) usado para processar os dados. O modelo é o resultado final gerado após o algoritmo treinar com os dados; é a representação computacional pronta para fazer previsões.**
3. Compare uma regra de bloqueio de palavra com um modelo de spam. **Uma regra de bloqueio (programação tradicional) é estática e bloqueia e-mails apenas se contiverem palavras específicas cadastradas manualmente (ex: "ganhe dinheiro"). Um modelo de spam (Machine Learning) analisa milhares de e-mails, descobre de forma autônoma quais combinações de palavras, remententes e metadados indicam spam, e se adapta a novas táticas de fraude sem depender de regras rígidas.**
4. Dê um exemplo de atributo e alvo e defina o momento da previsão. **Exemplo: Prever o preço de um carro usado. Os atributos (features) são as características do carro (ano, quilometragem, marca). O alvo (target) é o preço final de venda. O momento da previsão ocorre quando apresentamos ao modelo já treinado os atributos de um carro novo (não visto no treinamento), e ele calcula/prevê qual deverá ser o valor (alvo).**

## 2. Quadro comparativo

| Critério | Supervisionado | Não supervisionado |
| --- | --- | --- |
| Presença de alvo no treino | **Sim. Os dados vêm com o gabarito (rótulos ou valores conhecidos).** | **Não. Os dados são brutos, sem rótulos ou resultados pré-definidos.** |
| Pergunta que pode responder | **Qual é a categoria ou valor esperado para este novo dado?** | **Como esses dados estão organizados e existem padrões ou semelhanças ocultas neles?** |
| Tipo de saída | **Uma classe/categoria ou um valor numérico (contínuo).** | **Grupos (clusters) de dados similares ou associações.** |
| Tarefas e exemplo | **Classificação e Regressão. Ex: Prever se um paciente tem diabetes ou não.** | **Agrupamento (Clustering). Ex: Segmentar clientes de um supermercado por perfil de compra.** |
| Dados necessários | **Bases de dados contendo atributos independentes e uma variável alvo bem definida e confiável.** | **Bases de dados contendo apenas os atributos que caracterizam o objeto.** |
| Forma de avaliação | **Comparação das previsões com o gabarito (métricas como Acurácia, Precisão, Erro Quadrático Médio).** | **Medidas de coesão e separação dos grupos, e análise qualitativa feita por especialistas da área.** |
| Limitação | **Custo e esforço altos para construir bases rotuladas, podendo carregar vieses humanos.** | **Dificuldade de avaliar se o modelo está "certo" e de interpretar o significado real dos grupos encontrados.** |

## 3. Situações-problema

Os casos são cenários didáticos. Escolha **pelo menos cinco** e justifique sua classificação pelo significado do alvo ou por sua ausência.

| Caso | Situação-problema |
| --- | --- |
| 1 Educação | Prever evadiu/permaneceu com histórico rotulado de turmas anteriores. |
| 2 Saúde | Estimar o tempo de espera em minutos a partir de registros de atendimento. |
| 3 Comércio | Descobrir perfis de compra sem categorias prévias de clientes. |
| 4 Finanças | Prever inadimplente/adimplente com contratos e desfechos conhecidos. |
| 5 Segurança digital | Distinguir spam/legítima com mensagens já revisadas. |
| 6 Comércio | Estimar o total de vendas em reais na próxima semana. |
| 7 Educação | Agrupar perfis de acesso ao ambiente virtual sem rótulo de evasão. |
| 8 Saúde | Classificar exames em categorias previamente definidas por especialistas. |


| Caso escolhido | Alvo ou ausência de alvo | Categoria, quantidade ou grupos? | Abordagem e justificativa | Dados necessários e limitação |
| --- | --- | --- | --- | --- |
| **Caso 1: Educação (Prever evadiu/permaneceu)** | **Alvo: Status do aluno (evadiu ou permaneceu).** | **Categoria (Binária).** | **Aprendizado Supervisionado (Classificação). Existe um histórico anterior contendo as opções possíveis que ensina o modelo.** | **Histórico de notas, faltas e perfil com desfechos passados. Limitação: Necessita de histórico consistente e limpo.** |
| **Caso 2: Saúde (Estimar o tempo de espera em min)** | **Alvo: Tempo em minutos.** | **Quantidade (Contínua).** | **Aprendizado Supervisionado (Regressão). Busca-se prever um valor numérico baseando-se em eventos anteriores.** | **Registros de hora de chegada e início de atendimento. Limitação: Eventos não mapeados nos dados (ex: chuva forte, greves) podem gerar previsões erradas.** |
| **Caso 3: Comércio (Descobrir perfis de compra)** | **Ausência de alvo predefinido.** | **Grupos.** | **Aprendizado Não Supervisionado (Agrupamento). Busca encontrar divisões sem conhecer previamente as categorias.** | **Histórico de transações e horários dos clientes. Limitação: O significado de cada grupo exigirá análise humana interpretativa.** |
| **Caso 4: Finanças (Prever inadimplente/adimplente)** | **Alvo: Status do contrato.** | **Categoria (Binária).** | **Aprendizado Supervisionado (Classificação). O sistema usará os desfechos conhecidos de contratos antigos como gabarito.** | **Histórico de crédito e pagamentos com resultados. Limitação: Modelos treinados com dados antigos podem perpetuar vieses de crédito (ex: CEP).** |
| **Caso 6: Comércio (Estimar total de vendas em R$)** | **Alvo: Valor de vendas.** | **Quantidade (Contínua).** | **Aprendizado Supervisionado (Regressão). A meta é calcular um indicador numérico contínuo para o futuro.** | **Série temporal diária de volume de vendas e faturamento. Limitação: Vulnerável a mudanças bruscas no cenário econômico.** |

## 4. Raciocínio crítico

“Detectar fraude” define sozinho o tipo de aprendizado? Compare o uso de exemplos rotulados e a busca de operações atípicas sem rótulos.
**Não, o objetivo de negócio não define a abordagem por si só; o tipo de aprendizado depende dos dados disponíveis e de como abordamos o problema. Se tivermos um histórico onde auditores rotularam as transações passadas como "fraude" ou "legítima", usamos Aprendizado Supervisionado (Classificação). Por outro lado, se recebemos apenas um grande volume de transações sem nenhum rótulo histórico e pedimos ao algoritmo para procurar comportamentos que fogem do padrão normal da rede, usamos Aprendizado Não Supervisionado (Detecção de Anomalias).**

## Entrega

Salve este arquivo preenchido. Critérios: distinção conceitual, pertinência dos dados, justificativa e clareza. [Voltar ao início](README.md).
