<img src="assets/logo-udf.jpg" alt="UDF Centro Universitário" width="230">

**Profa. Kadidja Valéria | UDF**  
Ciência de Dados e Aprendizado de Máquina · Sistemas de Informação · 2026.2

# PBL integrador — Apoio à Permanência

## Identificação

- Estudante ou equipe: **Júlio César de Aguiar Nunes**
- Turma: **Sistemas de Informação - 2026.2**
- Data de realização: **30/10/2026**
- Integrantes e contribuições (se em equipe): **Júlio César de Aguiar Nunes**

## Contexto

Uma instituição quer compreender sinais associados à evasão e oferecer apoio antes do fim do semestre. Público-alvo: estudantes matriculados e equipe de apoio acadêmico. O cenário é didático e a prática usa dados sintéticos.

**Questão norteadora:** como identificar situações que merecem atenção e oferecer apoio, reconhecendo erros do modelo e dificuldades que os dados não capturam?

## Proposta da equipe

### 1. Delimitação

- Problema e público: **Altas taxas de abandono de curso. Público: Estudantes em risco (para receberem ajuda) e Equipe de Apoio/Tutoria (para direcionarem seus esforços limitados).**
- Decisão que o resultado pode apoiar: **Decidir quais alunos a equipe de tutoria deve contatar proativamente na metade do semestre para oferecer acompanhamento pedagógico ou psicológico voluntário.**
- Definição de evasão, unidade e horizonte: **Evasão: não renovação da matrícula ou abandono formal de todas as disciplinas. Unidade: 1 estudante matriculado. Horizonte: O desfecho no fim do semestre vigente.**

### 2. Perguntas de negócio

1. **Quais comportamentos acadêmicos (frequência, notas parciais, entregas) até a metade do semestre são os maiores indicativos de que um aluno abandonará o curso?**
2. **Como a coordenação pode priorizar as abordagens de apoio de forma eficiente, dado que o número de tutores é muito menor que o número de alunos?**

### 3. Dados necessários

| Dado | Papel no projeto | Disponível quando? | Origem pretendida | Necessidade e risco |
| --- | --- | --- | --- | --- |
| **Frequência nas aulas (%)** | **Atributo preditor** | **Semanalmente (até o meio do semestre)** | **Sistema Acadêmico / Diários** | **Necessário para medir engajamento. Risco: Atraso ou falha do professor ao registrar faltas.** |
| **Notas de avaliações parciais** | **Atributo preditor** | **Após o primeiro ciclo de provas (P1)** | **Portal do Aluno (LMS)** | **Necessário para avaliar desempenho. Risco: Notas não lançadas a tempo de rodar o modelo.** |
| **Status final (Evasão)** | **Variável-alvo (Gabarito)** | **Fechamento do semestre** | **Secretaria Acadêmica** | **Necessário para treinar o modelo. Risco: Confundir trancamento temporário (saúde/finanças) com abandono definitivo.** |

Diferencie dados usados na simulação e dados que seriam necessários para uma investigação real: **Os dados simulados são limpos, perfeitamente balanceados e contêm relações matemáticas exatas. No mundo real, precisaríamos lidar com dados ausentes (NaN), variáveis socioeconômicas (renda, se o aluno trabalha), tempo de deslocamento e fatores emocionais que não aparecem no boletim.**

### 4. Abordagem e algoritmo

- Classificação, regressão ou outra formulação, com justificativa: **Classificação Binária. Justificativa: Queremos categorizar o status final do aluno em duas classes discretas (0 = Permanece, 1 = Evade).**
- Algoritmo introdutório escolhido: **Árvore de Decisão (Decision Tree), por ser um modelo de "caixa branca" onde as regras e cortes (ex: faltas > 25%) são facilmente explicáveis para a equipe pedagógica.**
- Referência simples: **Dummy Classifier, prevendo sempre a classe majoritária (que o aluno vai permanecer), para termos uma linha de base (baseline) de comparação de acurácia.**

### 5. Avaliação

- Preparação, divisão e validação: **Divisão holdout (80% treino, 20% teste) isolando o teste desde o início. Validação cruzada (k=5) apenas nos dados de treino para calibrar a profundidade da árvore.**
- Métricas e razão da escolha: **A métrica principal não pode ser Acurácia, e sim Recall (Sensibilidade) da classe 1 (Evasão). O motivo é que o custo de um Falso Negativo (não identificar um aluno que vai evadir e perdê-lo) é muito maior que o de um Falso Positivo (oferecer apoio a quem não precisava).**
- Resultados efetivamente calculados e link da evidência: **Acurácia: 89% | Sensibilidade (Recall): 73,3%. Evidência disponível no arquivo [notebooks/pbl_integrador.ipynb](notebooks/pbl_integrador.ipynb).**
- Erros relevantes e consequências: **Falsos Positivos: A equipe de apoio perde tempo contatando um aluno que está bem, o que gera ineficiência. Falsos Negativos: O sistema acha que o aluno está seguro, ele não recebe apoio e abandona o curso (pior cenário).**

### 6. Limites e proteção

- Por que os resultados não demonstram causalidade? **O modelo aponta correlações. Por exemplo, "baixa frequência" e "evasão" estão ligados, mas a falta às aulas não é necessariamente a *causa* da evasão; a causa real pode ser o aluno ter arrumado um emprego no mesmo horário ou adoecido.**
- Por que dados sintéticos não validam uso institucional? **Porque eles não refletem a complexidade humana. Um modelo treinado em dados sintéticos falhará catastroficamente ao encontrar os ruídos e comportamentos imprevisíveis de alunos reais.**
- Que grupos podem ser mal representados? Como investigar isso com evidências apropriadas? **Alunos trabalhadores noturnos, mães/pais ou alunos de baixa renda podem ter padrões de acesso e notas diferentes, sendo falsamente taxados pelo modelo. Deve-se investigar aplicando métricas de *Fairness* (Equidade), fatiando os dados de teste por perfis demográficos para ver se a taxa de erro pune um grupo específico.**
- Como minimizar dados, restringir acesso e definir retenção? **Usar pseudonimização (trocar CPFs e Nomes por IDs criptografados), utilizar apenas dados estritamente acadêmicos (evitar dados sensíveis desnecessários), restringir acesso ao dashboard apenas para a coordenação, e descartar as predições a cada fim de ciclo.**
- Como garantir revisão humana e contestação? **O modelo nunca deve jubilar, trancar ou notificar o aluno automaticamente (sem "decisão automatizada"). O sistema deve gerar um alerta num painel para um tutor humano, que fará a curadoria e decidirá a abordagem.**

### 7. Recomendações de apoio

- Ação voluntária proposta: **Envio de um e-mail acolhedor pelo tutor (sem mencionar o algoritmo), convidando o aluno para um café ou uma sessão de mentoria voluntária para discutir plano de estudos.**
- Como acolher estudantes não sinalizados pelo modelo: **Manter plantões de dúvidas, oficinas de nivelamento e canais de escuta ativa abertos para toda a comunidade estudantil, divulgados amplamente, garantindo que quem não foi detectado possa buscar ajuda.**
- O que precisa ser verificado antes de aplicação real: **Aprovação pelo comitê de ética da instituição, revisão rigorosa de adequação à LGPD, treinamento da equipe de atendimento para não gerar pânico nos alunos e a realização de um teste piloto (A/B) em apenas um curso antes de expandir.**

Previsões não são certezas. Priorize apoio e revisão humana, sem punições ou decisões automáticas prejudiciais.

## Apresentação de cinco minutos

| Tempo | Conteúdo | Responsável |
| --- | --- | --- |
| 1 minuto | Problema e dados | **(Contextualização, perguntas de negócio e viabilidade dos dados)** |
| 2 minutos | Método e resultados | **(Explicação da Árvore de Decisão, métricas de Recall e matriz de confusão)** |
| 1 minuto | Limitações | **(Viés de correlação/causalidade e riscos com grupos vulneráveis)** |
| 1 minuto | Recomendações de apoio | **(Protocolo de intervenção humana e ética na aplicação)** |

## Contribuições

| Integrante | Contribuição verificável |
| --- | --- |
| **Júlio César de Aguiar Nunes** | **Construção do pipeline de dados, modelagem da Árvore de Decisão, extração das métricas (Recall) e planejamento do protocolo de apoio.Levantamento das perguntas de negócio, análise crítica de vieses e LGPD, delimitação das regras de negócio e estruturação das limitações.** |

[Voltar ao início](README.md).
