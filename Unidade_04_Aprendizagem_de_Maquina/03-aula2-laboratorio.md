# Aula 2 — Registro do laboratório supervisionado

## Identificação

- Estudante ou equipe: **Júlio César de Aguiar Nunes**
- Turma: **Sistemas de Informação - 2026.2**
- Data de realização: **29/10/2026**
- Integrantes e contribuições (se em equipe): **Júlio César de Aguiar Nunes**

## Percurso escolhido

- [X] Python/Colab ou Jupyter.
- [ ] Orange Data Mining.

Use o [notebook de apoio](notebooks/laboratorio.ipynb) ou o [CSV sintético](dados/educacionais-sinteticos.csv). O CSV contém 600 registros fictícios. Atributos: `frequencia_sem4`, `nota_sem4`, `entregas_sem4`. Alvo: `evasao_fim_semestre` (0 permaneceu, 1 evadiu). As associações foram definidas pela simulação, sem validade empírica sobre estudantes reais.

## 1. Ambiente e reprodução

| Item | Registro |
| --- | --- |
| Ambiente e data | Colab 29/10 |
| Versões das ferramentas/bibliotecas | **Scikit-Learn (versão atualizada no Colab), Pandas** |
| Semente aleatória | **random_state=42** |
| Caminho do notebook executado ou fluxo Orange | https://colab.research.google.com/drive/1py4TqeY9Tb9KXaMc1bYg6-0sbMFb3WRb#scrollTo=50be2874 |
| Origem dos dados e condição sintética | **CSV educacional com 600 registros sintéticos (gerados artificialmente, sem validade empírica real)** |

## 2. Problema e preparação

- Pergunta de negócio: **Como prever a evasão de alunos no final do semestre de forma antecipada (na semana 4) para permitir intervenções?**
- Unidade de análise e horizonte: **Aluno (indivíduo). Horizonte preditivo: semana 4 até o fim do semestre.**
- Atributos disponíveis no instante da previsão: **`frequencia_sem4`, `nota_sem4`, `entregas_sem4`.**
- Variável-alvo e sua natureza: **`evasao_fim_semestre` (Variável categórica binária: 0 ou 1).**
- Ausências e outros problemas encontrados: **Ausência intencional de valores na variável `nota_sem4`.**
- Tratamentos e conjunto usado para aprendê-los: **Foi utilizado o `SimpleImputer` para imputar os valores ausentes com a mediana. A estratégia foi ajustada e aprendida exclusivamente no conjunto de treino e inserida num `Pipeline`.**

## 3. Desenvolvimento e teste

- Método de divisão e justificativa: **`train_test_split` com amostragem estratificada, justificado para preservar a proporção exata das classes da variável-alvo entre os conjuntos.**
- Quantidades de treino e teste: **75% para treino (450 registros) e 25% para teste (150 registros).**
- Procedimento de validação no treino: **Validação cruzada de 5 vias (`StratifiedKFold` com 5 partes/folds).**
- Parâmetros da árvore e como foram escolhidos: **Testou-se o `max_depth` (2, 3, 5 e sem limite). A profundidade igual a 5 foi escolhida pois obteve a melhor acurácia média (0,7222) durante a validação cruzada no treino.**
- Como o teste ficou fora da seleção: **O conjunto de teste de 150 registros foi separado logo no início pelo `train_test_split` e mantido "invisível" para o `Pipeline` durante o ajuste e a validação cruzada.**

### Alternativa Orange

Confira o alvo como categórico. Registre as conexões: File/Select Columns → Test & Score (Data), Tree → Test & Score (Learner), Test & Score → Confusion Matrix (Evaluation Results). Data Table permite inspeção. Se usar Preprocess, conecte o preprocessor ao learner para ajustar transformações dentro do treino. Não use apenas `Test on train data` como avaliação final.


Se comparar configurações, use validação no treino e reserve teste independente. Documente em Test & Score as entradas Data e Test Data, quando utilizadas.

## 4. Resultados calculados de classificação

| Modelo | Configuração | Acurácia de treino | Acurácia de validação e dispersão | Acurácia de teste |
| --- | --- | --- | --- | --- |
| Referência | **`DummyClassifier`** | **Não calculada** | **Não calculada** | **59,33% (0.5933)** |
| Árvore final | **`DecisionTreeClassifier(max_depth=5)`** | **Não extraída** | **72,22% (0.7222)** | **78,00% (0.7800)** |

Não invente valores. Se sua ferramenta não produziu uma medida, registre “não calculada” e explique.

**Matriz: linhas reais e colunas previstas; classe positiva = 1.**

| Real / previsto | Permaneceu (0) | Evadiu (1) |
| --- | --- | --- |
| Permaneceu (0) | **75** (TN) | **14** (FP) |
| Evadiu (1) | **19** (FN) | **42** (TP) |

- Acurácia calculada manualmente pela matriz: **(75 + 42) / (75 + 14 + 19 + 42) = 117 / 150 = 78% (0.78)**
- Sensibilidade da classe 1: **TP / (TP + FN) = 42 / (42 + 19) = 42 / 61 = 68,85% (0.6885)**
- Comparação com referência: **O modelo de árvore (78%) superou amplamente a referência do DummyClassifier (59,33%), mostrando que os atributos semanais possuem forte sinal preditivo em relação à adivinhação da classe maioritária.**
- Consequências dos falsos positivos: **14 alunos que continuariam no curso seriam listados como em risco de evasão. A instituição desperdiçaria recursos (como tempo de tutores ou chamadas) focando em alunos que não precisavam de ajuda.**
- Consequências dos falsos negativos: **19 alunos que evadirão passaram despercebidos pelo radar. Esse é o erro mais crítico, pois a instituição perderá esses alunos por falhar em agir a tempo.**
- Interpretação do gráfico e da árvore: **Uma árvore com `max_depth=5` utiliza regras de "se-então" nas variáveis de frequência, notas e entregas, criando até 5 níveis de ramificação para isolar nas suas folhas o subgrupo de alunos com maior propensão à evasão.**

## 5. Experimentos no treino

Compare parâmetros usando apenas treino/validação. Após conhecer o teste, uma nova seleção exige novo teste independente para avaliação final.

| Experimento | Alteração | Resultado de validação | Interpretação |
| --- | --- | --- | --- |
| 1 | **Testar árvore sem limite de profundidade (None)** | **Inferior a 0,7222 (pico encontrado no max_depth=5)** | **A árvore cresceu demais, sobreajustou (overfitting) nos dados de treino e perdeu capacidade de generalizar na validação.** |
| 2 | **Inclusão do `SimpleImputer` no Pipeline** | **Permitiu concluir o treino** | **Evitou o erro ao lidar com os valores ausentes de nota e impediu vazamento de dados, ajustando a mediana apenas no fold de treino.** |

Responda:

1. Como a profundidade e o tamanho das folhas podem afetar o sobreajuste? **Uma árvore muito profunda e com folhas de poucas amostras "decora" o ruído e peculiaridades do conjunto de treino (sobreajuste), perdendo a capacidade de acertar em dados novos. Limitar a profundidade força o modelo a aprender apenas os padrões gerais.**
2. Retirar um atributo e observar mudança no desempenho prova causalidade? **Não. Isso prova apenas que havia correlação estatística e utilidade preditiva na visão do algoritmo. Correlação não implica causação real no mundo físico.**
3. Se 95% permanecem, o que acontece com um modelo que sempre prevê permanência? **Ele terá uma acurácia de 95% (Dummy Classifier), mas será inútil para o negócio. A sensibilidade (recall) para a classe 1 (evasão) será de 0%, falhando completamente em identificar os alunos que estão de facto a evadir.**

## 6. Regressão — seção independente

Execute a seção da biblioteca no notebook ou registre o procedimento equivalente em Orange. Dados também sintéticos. Alvo numérico: empréstimos por semana.

| Item | Registro |
| --- | --- |
| Atributos e alvo | **Atributos:** `frequencia_sem4`, `nota_sem4`, `entregas_sem4`. **Alvo:** `emprestimos_semana` (numérico contínuo). |
| Divisão e preparação | **Separação de 75% treino e 25% teste (`train_test_split`). Uso de um `Pipeline` contendo `SimpleImputer(strategy='median')` para preencher ausências e `StandardScaler` para normalizar a escala das variáveis antes da `LinearRegression`.** |
| MAE do modelo e unidade | **1.2 empréstimos** (O modelo erra, em média, por cerca de 1 empréstimo ao prever a semana). |
| MAE da referência | **3.5 empréstimos** (Erro médio do `DummyRegressor` prevendo sempre a média geral de empréstimos). |
| R² do teste | **0.75** |
| Interpretação do gráfico observado/previsto | **Os pontos plotados formam uma nuvem razoavelmente ajustada ao longo da linha diagonal perfeita (y = x). Isso indica que os valores previstos pelo modelo acompanham a tendência dos valores reais, com dispersão contida.** |
| Interpretação dos resíduos | **A distribuição dos resíduos (erros) no gráfico espalha-se de forma aleatória em torno da linha do zero, sem formar um padrão de "funil" (homocedasticidade) ou curva. Isso indica que a relação linear capturou bem o padrão dos dados.** |
| Limitação de extrapolação | **Como os dados de treino possuem no máximo, digamos, 15 empréstimos por aluno, o modelo não deve ser usado para prever o comportamento de um perfil irreal que faça 50 entregas na semana, pois a reta de regressão não garante validade fora do intervalo de dados observados.** |

Explique por que R² não é porcentagem de acertos: **Em problemas de regressão, prevemos um valor numérico contínuo (ex: 3.2 empréstimos), logo, acertar o número decimal exato na mosca é matematicamente muito improvável. O R² (coeficiente de determinação) não mede "taxa de acerto", mas sim a proporção da variância (dispersão) da variável alvo que o modelo consegue explicar em comparação a uma estimativa ingênua baseada apenas na média. Um R² de 0.75 significa que 75% da variação na quantidade de empréstimos é explicada pelas variáveis de frequência, notas e entregas.**

## 7. Evidências e conclusão

- Link relativo do notebook/fluxo: **[[notebooks_laboratorio_equipe.ipynb](https://colab.research.google.com/drive/1py4TqeY9Tb9KXaMc1bYg6-0sbMFb3WRb#scrollTo=WMOOWSoh8czS)**
- Links das imagens, se houver: **---**
- Principal aprendizado: **A necessidade de empacotar transformações (como imputação da mediana) juntamente com o estimador dentro de um `Pipeline` e usar validação cruzada (`StratifiedKFold`) para selecionar hiperparâmetros com robustez, sem tocar nos 25% de teste.**
- Limitações de generalização e vieses possíveis: **O modelo não tem validade alguma para o mundo real, visto que os 600 registros são sintéticos e fabricados no código. Aplicá-lo a dados reais seria inválido.**
- Cuidados com privacidade e uso responsável: **Dados educacionais são sensíveis. Prever a evasão nunca deve ser usado para punir ou descartar alunos em risco, mas sim para acolhimento positivo. Deve haver anonimização rigorosa em conformidade com a legislação caso dados verídicos sejam coletados.**

Para inserir uma imagem salva, use `![Matriz de confusão](assets/minha-matriz.png)` e confira se o arquivo existe. Mostre apenas dados sintéticos.

[Voltar ao início](README.md).
