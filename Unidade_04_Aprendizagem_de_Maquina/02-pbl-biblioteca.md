<img src="assets/logo-udf.jpg" alt="UDF Centro Universitário" width="230">

**Profa. Kadidja Valéria | UDF**  
Ciência de Dados e Aprendizado de Máquina · Sistemas de Informação · 2026.2

# PBL — Biblioteca em Foco

## Identificação

- Estudante ou equipe: **Júlio César de Aguiar Nunes**
- Turma: **Sistemas de Informação - 2026.2**
- Data de realização: **29/10/2026**
- Integrantes e contribuições (se em equipe): **Júlio César de Aguiar Nunes**

## Situação-problema

Uma biblioteca universitária enfrenta filas em horários específicos e baixa utilização de alguns serviços. A coordenação deseja melhorar o atendimento e planejar recursos. Este é um cenário didático.

**Público-alvo:** estudantes usuários, equipe de atendimento e gestão.

**Questão norteadora:** como os dados podem orientar o planejamento da biblioteca e a oferta de serviços sem identificar ou discriminar seus usuários?

## Investigação em equipe

1. Delimitem o problema e a decisão a apoiar.
2. Registrem o que já sabem e o que precisam investigar.
3. Formulem duas perguntas de negócio.
4. Definam a unidade de análise, os atributos e o alvo, quando houver.
5. Justifiquem a abordagem e indiquem se uma análise descritiva já seria suficiente.
6. Proponham avaliação, cuidados com os dados e limitações.

## Canvas da equipe

| Campo | Registro |
| --- | --- |
| Problema e decisão | **Problema: Filas concentradas e ociosidade de certos serviços. Decisão: Ajustar a escala de horários dos funcionários e reposicionar os serviços oferecidos.** |
| Público-alvo | **Estudantes (para usufruir de um serviço mais rápido), Equipe de atendimento (para melhor divisão de carga de trabalho) e Gestão (para otimizar custos).** |
| O que sabemos | **Ocorrem picos de filas em horários específicos (gargalos) e alguns recursos dispõem de infraestrutura, mas têm baixa adoção pelos alunos.** |
| O que precisamos investigar | **Quais variáveis (horário, dia, proximidade de provas) afetam o fluxo; quais serviços estão ociosos e se os serviços são usados em conjunto.** |
| Pergunta de negócio 1 | **Qual será o volume esperado de estudantes na biblioteca em um dado dia e horário, para que possamos dimensionar a equipe de atendimento?** |
| Pergunta de negócio 2 | **Quais serviços da biblioteca costumam ser acessados em conjunto pelos alunos, para que possamos criar campanhas de divulgação integradas?** |
| Unidade de análise e período | **Unidade: Bloco de 1 hora de funcionamento. Período: Histórico de acessos dos últimos 4 semestres letivos (2 anos).** |
| Atributos, unidades e origem pretendida | **Atributos: Dia da semana (categórico), Horário (hora), Semana de Provas (booleano - sim/não), Condição Climática (categórico). Origem: Registros de catracas e Calendário Acadêmico.** |
| Alvo e momento em que será conhecido, ou ausência de alvo | **Alvo: Quantidade total de estudantes na biblioteca (valor numérico contínuo). O momento em que será conhecido é no fim de cada hora registrada na catraca.** |
| Abordagem e justificativa | **Para o fluxo: Aprendizado Supervisionado (Regressão), pois queremos prever uma quantidade numérica baseada no histórico. Para os serviços: Aprendizado Não Supervisionado (Regras de Associação/Agrupamento) pois não há alvo, apenas queremos descobrir padrões.** |
| Alternativa sem ML | **Uma análise descritiva através de *dashboards* mostrando a média histórica de alunos por horário/dia já seria muito útil para embasar a realocação da escala inicial.** |
| Como avaliar a proposta | **Comparando as previsões numéricas do modelo de regressão com as contagens reais das catracas (usando métricas de erro como o MAE/RMSE) e monitorando se o tempo médio de espera na fila diminuiu.** |
| Privacidade e limitações | **Privacidade: Remoção total de chaves de identificação (Matrícula, RA, Nome) da base de dados, utilizando apenas totalizadores agregados. Limitações: O modelo não preverá eventos atípicos (greves, eventos extraordinários no campus).** |

## Registro de colaboração

| Integrante | Papel | Contribuição |
| --- | --- | --- |
| **Eduardo Salgueiro** | **Engenharia de Dados e Modelagem** | **Definição técnica dos atributos, variável alvo, métricas de avaliação do modelo preditivo e estruturação de dados.** |
| **Julio Cesar** | **Análise de Negócios e Requisitos** | **Formulação das perguntas de negócio, delimitação do problema e restrições de privacidade e ética.** |

## Conclusão da equipe

**Proposta:** Desenvolver um modelo preditivo de fluxo de alunos baseado no horário, dia e calendário acadêmico, aliado a relatórios de serviços agrupados, visando reestruturar as escalas de trabalho e campanhas sem identificar nenhum usuário de forma individual.

**Dúvida ainda aberta:** Como podemos mensurar o uso de serviços de "livre acesso" da biblioteca (como a ocupação de sofás e mesas abertas) que não exigem registro em sistema ou empréstimo formal?

**Feedback recebido e melhoria realizada:** No debate inicial, cogitou-se usar o histórico de livros de cada aluno para ver quem usava os serviços. O feedback apontou risco de violação de privacidade (discriminação de leitura). A melhoria foi agregar os dados de empréstimo apenas por "área do conhecimento" (ex: TI, Direito) e faixa horária, garantindo o anonimato total da unidade de análise.

## Entrega

Canvas preenchido e síntese para apresentação de dois minutos. Avaliar coerência da pergunta, dados, abordagem, responsabilidade e colaboração. [Voltar ao início](README.md).
