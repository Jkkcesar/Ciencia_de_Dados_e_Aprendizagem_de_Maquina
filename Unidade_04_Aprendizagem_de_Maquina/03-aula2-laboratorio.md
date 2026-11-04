<img src="assets/logo-udf.jpg" alt="UDF Centro Universitário" width="230">

**Profa. Kadidja Valéria | UDF**  
Ciência de Dados e Aprendizado de Máquina · Sistemas de Informação · 2026.2

# Aula 2 — Registro do laboratório supervisionado

## Identificação

- Estudante ou equipe: **PREENCHER**
- Turma: **PREENCHER**
- Data de realização: **PREENCHER**
- Integrantes e contribuições (se em equipe): **PREENCHER**

## Percurso escolhido

- [ ] Python/Colab ou Jupyter.
- [ ] Orange Data Mining.

Use o [notebook de apoio](notebooks/laboratorio.ipynb) ou o [CSV sintético](dados/educacionais-sinteticos.csv). O CSV contém 600 registros fictícios. Atributos: `frequencia_sem4`, `nota_sem4`, `entregas_sem4`. Alvo: `evasao_fim_semestre` (0 permaneceu, 1 evadiu). As associações foram definidas pela simulação, sem validade empírica sobre estudantes reais.

## 1. Ambiente e reprodução

| Item | Registro |
| --- | --- |
| Ambiente e data | PREENCHER |
| Versões das ferramentas/bibliotecas | PREENCHER |
| Semente aleatória | PREENCHER |
| Caminho do notebook executado ou fluxo Orange | PREENCHER |
| Origem dos dados e condição sintética | PREENCHER |

## 2. Problema e preparação

- Pergunta de negócio: **PREENCHER**
- Unidade de análise e horizonte: **PREENCHER**
- Atributos disponíveis no instante da previsão: **PREENCHER**
- Variável-alvo e sua natureza: **PREENCHER**
- Ausências e outros problemas encontrados: **PREENCHER**
- Tratamentos e conjunto usado para aprendê-los: **PREENCHER**

## 3. Desenvolvimento e teste

- Método de divisão e justificativa: **PREENCHER**
- Quantidades de treino e teste: **PREENCHER**
- Procedimento de validação no treino: **PREENCHER**
- Parâmetros da árvore e como foram escolhidos: **PREENCHER**
- Como o teste ficou fora da seleção: **PREENCHER**

### Alternativa Orange

Confira o alvo como categórico. Registre as conexões: File/Select Columns → Test & Score (Data), Tree → Test & Score (Learner), Test & Score → Confusion Matrix (Evaluation Results). Data Table permite inspeção. Se usar Preprocess, conecte o preprocessor ao learner para ajustar transformações dentro do treino. Não use apenas `Test on train data` como avaliação final.

**Fluxo e método de avaliação realmente utilizados:** PREENCHER

Se comparar configurações, use validação no treino e reserve teste independente. Documente em Test & Score as entradas Data e Test Data, quando utilizadas.

## 4. Resultados calculados de classificação

| Modelo | Configuração | Acurácia de treino | Acurácia de validação e dispersão | Acurácia de teste |
| --- | --- | --- | --- | --- |
| Referência | PREENCHER | PREENCHER | PREENCHER | PREENCHER |
| Árvore final | PREENCHER | PREENCHER | PREENCHER | PREENCHER |

Não invente valores. Se sua ferramenta não produziu uma medida, registre “não calculada” e explique.

**Matriz: linhas reais e colunas previstas; classe positiva = 1.**

| Real / previsto | Permaneceu (0) | Evadiu (1) |
| --- | --- | --- |
| Permaneceu (0) | PREENCHER (TN) | PREENCHER (FP) |
| Evadiu (1) | PREENCHER (FN) | PREENCHER (TP) |

- Acurácia calculada manualmente pela matriz: **PREENCHER**
- Sensibilidade da classe 1: **PREENCHER**
- Comparação com referência: **PREENCHER**
- Consequências dos falsos positivos: **PREENCHER**
- Consequências dos falsos negativos: **PREENCHER**
- Interpretação do gráfico e da árvore: **PREENCHER**

## 5. Experimentos no treino

Compare parâmetros usando apenas treino/validação. Após conhecer o teste, uma nova seleção exige novo teste independente para avaliação final.

| Experimento | Alteração | Resultado de validação | Interpretação |
| --- | --- | --- | --- |
| 1 | PREENCHER | PREENCHER | PREENCHER |
| 2 | PREENCHER | PREENCHER | PREENCHER |

Responda:

1. Como a profundidade e o tamanho das folhas podem afetar o sobreajuste? **PREENCHER**
2. Retirar um atributo e observar mudança no desempenho prova causalidade? **PREENCHER**
3. Se 95% permanecem, o que acontece com um modelo que sempre prevê permanência? **PREENCHER**

## 6. Regressão — seção independente

Execute a seção da biblioteca no notebook ou registre o procedimento equivalente em Orange. Dados também sintéticos. Alvo numérico: empréstimos por semana.

| Item | Registro |
| --- | --- |
| Atributos e alvo | PREENCHER |
| Divisão e preparação | PREENCHER |
| MAE do modelo e unidade | PREENCHER |
| MAE da referência | PREENCHER |
| R² do teste | PREENCHER |
| Interpretação do gráfico observado/previsto | PREENCHER |
| Interpretação dos resíduos | PREENCHER |
| Limitação de extrapolação | PREENCHER |

Explique por que R² não é porcentagem de acertos: **PREENCHER**

## 7. Evidências e conclusão

- Link relativo do notebook/fluxo: **PREENCHER**
- Links das imagens, se houver: **PREENCHER**
- Principal aprendizado: **PREENCHER**
- Limitações de generalização e vieses possíveis: **PREENCHER**
- Cuidados com privacidade e uso responsável: **PREENCHER**

Para inserir uma imagem salva, use `![Matriz de confusão](assets/minha-matriz.png)` e confira se o arquivo existe. Mostre apenas dados sintéticos.

[Voltar ao início](README.md).
