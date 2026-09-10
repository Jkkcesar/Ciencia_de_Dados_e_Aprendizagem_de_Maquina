# Atividade Prática — Detetives dos Dados

## Ciência de Dados e Aprendizagem de Máquina — Aula 01

**Tema:** Introdução à Ciência de Dados e Big Data  
**Metodologia:** Trabalho em equipe  
**Tempo:** 20 minutos  
**Entregável:** Mapa do Problema de Ciência de Dados

## 1\. Identificação da equipe

| Campo             | Resposta                      |
| ----------------- | ----------------------------- |
| **Turma:**        |                               |
| ---               | ---                           |
| **Data:**         | 25/08/2026                    |
| ---               | ---                           |
| **Equipe:**       |                               |
| ---               | ---                           |
| **Integrante 1:** | Eduardo Salgueiro Evangelista |
| ---               | ---                           |
| **Integrante 2:** | Julio Cesar de Aguiar Nunes   |
| ---               | ---                           |
| **Integrante 3:** |                               |
| ---               | ---                           |
| **Integrante 4:** |                               |
| ---               | ---                           |
| **Integrante 5:** |                               |
| ---               | ---                           |

## 2\. Objetivo da atividade

Nesta atividade, sua equipe deverá analisar um **problema real** e pensar como uma equipe de Ciência de Dados poderia utilizar dados para compreender a situação e apoiar uma decisão.

O objetivo não é desenvolver um sistema ou modelo de Machine Learning neste momento.

O objetivo é aprender a **pensar como um cientista de dados**:

**Problema → Dados → Informação → Análise → Decisão → Benefício**

## 3\. Escolha do problema

Escolha uma área para investigar:

- Comércio
- Banco
- Saúde
- Transporte
- Educação
- Entretenimento
- Indústria
- Meio ambiente
- Esportes

X Outra: Assistência social/ Terceiro setor.

### Problema escolhido

**Descreva, em poucas linhas, o problema que sua equipe pretende analisar.**

Escreva aqui:

o descarte irregular de bens de consumo em áreas urbanas, aliados à dificuldade logística e estrutural de redistribuir materiais ainda úteis para instituições de caridade.

## 4\. Quem possui esse problema?

Identifique a organização, grupo ou público afetado pelo problema.

**Quem possui ou enfrenta esse problema?**

Pessoas físicas (doadores) que possuem itens excedentes e não sabem como doar, e Instituições de caridade (donatários) que enfrentam carência de recursos materiais.

### Quem é afetado pelo problema?

A sociedade, o Terceiro Setor que depende da economia da solidariedade e o meio ambiente.

## 5\. Por que esse problema é importante?

Explique por que vale a pena investigar esse problema utilizando dados.

**Qual é o impacto do problema?**

Organizar a redistribuição de resíduos sólidos que ainda tem utilidade para reduzir o impacto ambiental do descarte e ajuda no sustento das instituições cadastradas.

## 6\. Qual decisão precisa ser tomada?

Imagine que sua equipe foi contratada para ajudar uma organização.

**Qual decisão a organização precisa tomar?**

Desenvolver uma plataforma que tem como objetivo, de forma segura e logística, conectar um item oferecido pelo doador com a organização certa. Além disso, a administração precisa decidir sobre a validação (aprovação, reprovação ou suspensão) das instituições para garantir que as doações sejam seguras e sem fraudes.

# 7\. Identificação dos dados

Agora pense:

**Quais dados seriam necessários para compreender esse problema?**

Liste pelo menos **5 dados**.

| Nº  | Dado necessário                                          | Por que esse dado é importante?                                                                                                            |
| --- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | Endereço (Bairro, Cidade etc.)                           | Serve para calcular a proximidade entre a necessidade da instituição e o doador, possibilitando organizar a retirada no local ou entrega.  |
| --- | ---                                                      | ---                                                                                                                                        |
| 2   | Estado de conservação e categoria do donativo.           | Permite que a instituição avalie previamente se o bem oferecido atende às suas carências operacionais antes de aceitar.                    |
| --- | ---                                                      | ---                                                                                                                                        |
| 3   | Prioridade da necessidade (Baixa, Média, Alta, Urgente). | Orienta a plataforma a destacar e ordenar demandas urgentes, canalizando a solidariedade para onde o impacto é mais necessário no momento. |
| --- | ---                                                      | ---                                                                                                                                        |
| 4   | Status de validação do cadastro institucional.           | É crítico para assegurar que apenas organizações legítimas (verificadas pela administração) recebam os donativos.                          |
| --- | ---                                                      | ---                                                                                                                                        |
| 5   | Avaliações, histórico de recebimento e notas.            | Garante transparência e rastreabilidade dos usuários, auditando se o que foi prometido foi entregue e correspondia à descrição.            |
| --- | ---                                                      | ---                                                                                                                                        |

### Exemplos

Podem ser considerados dados como:

- idade;
- localização;
- frequência;
- notas;
- compras;
- valores;
- horários;
- avaliações;
- histórico de utilização;
- registros de atendimento;
- imagens;
- textos;
- localização geográfica.

# 8\. Que informações queremos descobrir?

Os dados, quando analisados, podem gerar informações úteis.

**O que sua equipe gostaria de descobrir a partir dos dados?**

### Pergunta 1

Quais instituições validadas estão mais próximas da localização do doador?

### Pergunta 2

Quais são as necessidades urgentes e de alta prioridade cadastradas no sistema que ainda não foram atendidas?

### Pergunta 3

Qual a melhor rota ou viabilidade para o agendamento de entrega a partir das condições (retirada no local, entrega pelo doador, etc.)?

### Pergunta 4

Qual é o nível de engajamento e a confiabilidade de cada doador ou instituição através de seu histórico de recebimentos e notas atribuídas?

# 9\. Quais padrões podemos procurar?

Pense como um cientista de dados.

Sua equipe poderia procurar:

- Tendências

X Comparações

- Grupos semelhantes
- Comportamentos recorrentes
- Valores fora do padrão

X Relações entre variáveis

- Mudanças ao longo do tempo
- Outros: \_**\_**\_**\_**\_**\_**\_**\_**\__

### Explique um padrão que vocês gostariam de encontrar

Podemos procurar relações entre as categorias de donativos ofertadas e os interesses cadastrados, analisando geograficamente a proximidade. Isso ajuda a padronizar cruzamentos que transformam pedidos institucionais em doações efetuadas.

# 10\. Qual análise poderia ser realizada?

Como os dados poderiam ser analisados?

Marque uma ou mais possibilidades:

- Análise descritiva

X Comparação entre grupos

- Análise temporal
- Visualização por gráficos

X Identificação de padrões

- Classificação
- Previsão
- Agrupamento
- Outra: \_**\_**\_**\_**\_**\_**\_**\_**\__

### Explique

Uma análise de correlação local, comparando o grupo de doadores e o grupo de instituições. A plataforma cruzaria necessidades listadas auxiliando na escolha de para quem doar e apresentando um mapa para planejar as entregas.

# 11\. Qual decisão poderia ser tomada?

Depois de analisar os dados, imagine que sua equipe encontrou informações importantes.

**Que decisão poderia ser tomada com base nos resultados?**

Com os dados reunidos, a administração pode usar os dados operacionais na tomada de decisão nos processos da plataforma, como por exemplo:

a decisão de formalizar um agendamento e abrir uma comunicação via chat para acompanhar a entrega com base nos interesses em comum.

analisar a veracidade das instituições e a proximidade logística para realizar o bloqueio de contas em caso de irregularidade.

# 12\. Qual seria o benefício?

Qual seria o possível benefício da decisão para a organização ou para as pessoas envolvidas?

O benefício direto seria organizar o fluxo de doações, eliminar as barreiras logísticas de encontrar quem precisa de itens ociosos, fomentar o descarte consciente e fortalecer o Terceiro Setor e a economia da solidariedade no Brasil.

# 13\. Os 5 Vs do Big Data

Analise o problema escolhido pela equipe.

| V              | Pergunta                                                   | Resposta da equipe                                                                                                               |
| -------------- | ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Volume**     | Existe uma grande quantidade de dados?                     | Sim, há acúmulo contínuo de cadastros, catálogo de donativos, registros de agendamentos e logs de auditoria e protocolos.        |
| ---            | ---                                                        | ---                                                                                                                              |
| **Velocidade** | Os dados são gerados ou processados rapidamente?           | A criação de agendamentos e o sistema de mensagens instantâneas do chat exigem processamento e notificações quase em tempo real. |
| ---            | ---                                                        | ---                                                                                                                              |
| **Variedade**  | Existem diferentes tipos ou formatos de dados?             | Sim, o sistema processa textos estruturados, coordenadas geográficas, imagens (fotos de donativos) e categorias.                 |
| ---            | ---                                                        | ---                                                                                                                              |
| **Veracidade** | Os dados podem apresentar erros ou problemas de qualidade? | Sim. Para contornar, a plataforma exige aprovação manual do CNPJ e razões sociais das instituições pelo administrador.           |
| ---            | ---                                                        | ---                                                                                                                              |
| **Valor**      | Os dados podem gerar algum benefício ou apoiar decisões?   | Alto valor. Os dados são usados para conectar o doador com a instituição, em sustento e amparo para o Terceiro Setor.            |
| ---            | ---                                                        | ---                                                                                                                              |

### Qual dos 5 Vs é mais relevante para o problema?

A Veracidade

### Justifique

A segurança e a confiança são os pilares de uma plataforma de doação. Se o sistema não auditar e aprovar ativamente o status legal (CNPJ, razão social) e as ações dos recebedores, os doadores podem se tornar vítimas de golpes. A veracidade garante o direcionamento efetivo da ajuda solidária.

# 14\. Mapa do Problema de Ciência de Dados

Complete o fluxo abaixo:

PROBLEMA  
Desperdício de Resíduos uteis.  
barreiras logísticas para doar às organizações de assistência social.  
───────────┬─────────────────────────────────────────────────────────  
↓  
<br/>DADOS  
status de aprovação de instituições.  
informações geolocalizadas etc.  
───────────┬────────────────────────  
↓  
<br/>INFORMAÇÕES  
Ofertas e demandas organizadas por localidade.  
urgência de necessidade e status de validação.  
───────────┬──────────────────────────────────  
↓  
<br/>ANÁLISE  
Verificação contínua das instituições válidas.  
cruzamento entre interesse do donatário e a oferta do doador.  
───────────┬──────────────────────────────────────────────────  
↓  
<br/>DECISÃO  
Aprovar e mediar a conexão entre partes.  
abrindo fluxos de chat interno e agendamento de entregas precisas.  
───────────┬──────────────────────────────────────────────────────  
↓  
<br/>BENEFÍCIO  
Reaproveitamento sustentável dos materiais.  
fortalecimento eficiente das instituições de caridade.  
─────────────────────────────────────────────────────

### Resuma cada etapa

**Problema:**  
\>

**Dados:**  
\>

**Informação:**  
\>

**Análise:**  
\>

**Decisão:**  
\>

**Benefício:**  
\>

# 15\. Preparação para apresentação

A equipe terá **2 minutos** para apresentar sua proposta.

Organizem a apresentação seguindo esta estrutura:

### Nosso problema

É a dificuldade logística e a falta de informação que impedem o reaproveitamento e a doação de bens úteis, gerando descarte irregular e privando instituições de caridade de materiais necessários.

### Precisamos destes dados

Localização espacial (endereço/cidade), categorias e conservação dos donativos, nível de prioridade (urgência) das necessidades e o status de validação do cadastro da instituição.

### 3\. Queremos descobrir

Quais são as instituições seguras mais próximas do doador que possuem necessidades compatíveis com o item oferecido.

### 4\. Pretendemos analisar

O cruzamento geográfico e a correlação de interesses entre o catálogo de ofertas de pessoas físicas e o painel de necessidades das organizações.

### 5\. A decisão poderia ser

A plataforma deve recomendar a conexão exata entre as partes e intermediar a abertura de um chat e um protocolo de agendamento logístico para a entrega de donativos.

### 6\. O benefício esperado é

Reduzir o desperdício, promover o descarte consciente e garantir que os recursos cheguem com agilidade ao Terceiro Setor.

# 16\. Checklist da equipe

Antes de entregar, confira:

X Definimos um problema real.

X Identificamos quem é afetado pelo problema.

X Explicamos por que o problema é importante.

X Identificamos pelo menos 5 dados necessários.

X Definimos perguntas que queremos responder.

X Identificamos possíveis padrões.

X Indicamos como os dados poderiam ser analisados.

X Definimos uma possível decisão.

X Identificamos o benefício esperado.

X Analisamos os 5 Vs do Big Data.

X Preenchemos o Mapa do Problema.

X Estamos preparados para apresentar em 2 minutos.

# 17\. Reflexão final

Responda individualmente ou em equipe:

**Ter muitos dados significa necessariamente tomar boas decisões? Por quê?**

**Resposta:**

Não. Ter um grande volume de doadores e entidades não traria qualquer eficácia para a doação solidária se não houvesse governança. Os dados apenas apoiam a tomada de boa decisão porque passam por fluxos que garantem sua confiabilidade, como a necessidade de ter o cadastro validado por administradores, além da padronização dos dados no agendamento e chat, que transformam dados complexos em ações transparentes.

## Entrega

### Produto final

A equipe deverá entregar:

**Mapa do Problema de Ciência de Dados**

contendo:

Problema  
↓  
Dados necessários  
↓  
Informações desejadas  
↓  
Análise  
↓  
Decisão  
↓  
Benefício esperado

**Formato sugerido:** Markdown, PDF ou documento disponibilizado pelo professor.

**Apresentação:** 2 minutos por equipe.
