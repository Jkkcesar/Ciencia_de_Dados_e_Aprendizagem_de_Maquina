<img src="assets/logo-udf.jpg" alt="UDF Centro Universitário" width="230">

**Profa. Kadidja Valéria | UDF**  
Ciência de Dados e Aprendizado de Máquina · Sistemas de Informação · 2026.2

# Questões de revisão — Registro individual

## Identificação

- Estudante ou equipe: **Júlio César de Aguiar Nunes**
- Turma: **Sistemas de Informação - 2026.2**
- Data de realização: **30/10/2026**
- Integrantes e contribuições (se em equipe): **Júlio César de Aguiar Nunes**

Responda com suas palavras e apresente os cálculos quando solicitados. Os números dos exercícios manuais não são os resultados do laboratório.

## 1. Explique a diferença entre regra programada para bloquear uma palavra e modelo de spam treinado com exemplos.

**Resposta:** Uma regra programada é estática e depende de um humano escrever uma condição explícita no código (ex: `se conter "grátis", bloqueie`). Já um modelo treinado com exemplos (Machine Learning) não recebe regras explícitas; ele analisa milhares de e-mails rotulados, aprende sozinho padrões complexos (combinações de palavras, metadados) e se ajusta a novas táticas de spam automaticamente.

## 2. Por que codificar evadiu/permaneceu como 1/0 não transforma classificação em regressão?

**Resposta:** Porque 1 e 0 nesse contexto são apenas rótulos categóricos (classes) que representam estados, não valores numéricos contínuos onde a distância ou escala importa. O modelo continua calculando a probabilidade de uma amostra pertencer a uma classe ou outra, em vez de estimar uma quantidade infinita.

## 3. Uma biblioteca quer estimar empréstimos e descobrir perfis. Que tarefas correspondem a cada pergunta?

**Resposta:** "Estimar empréstimos" (uma quantidade numérica) corresponde à tarefa de **Regressão** (Aprendizado Supervisionado). "Descobrir perfis" (agrupar leitores com comportamentos similares sem categorias prévias) corresponde a **Agrupamento/Clustering** (Aprendizado Não Supervisionado).

## 4. O que são atributo, alvo e unidade de análise no laboratório educacional?

**Resposta:** 
- **Unidade de análise:** É o nível de observação da base, ou seja, 1 estudante.
- **Atributo:** São as variáveis ou características usadas para fazer a previsão (ex: notas parciais, faltas, entregas).
- **Alvo:** É a variável que queremos descobrir ou prever com base nos atributos (ex: se o aluno evadiu ou permaneceu).

## 5. Por que calcular a mediana antes de dividir os dados pode invalidar a avaliação?

**Resposta:** Porque gera "vazamento de dados" (Data Leakage). Se você calcula a mediana com a base toda e usa isso para preencher dados ausentes, informações do conjunto de teste estarão influenciando os dados de treino. O modelo acaba "espiando" o teste, resultando em métricas de avaliação ilusórias. O correto é dividir primeiro e calcular a mediana apenas com o treino.

## 6. Em uma matriz didática com TN=80, FP=5, FN=10, TP=5, calcule acurácia e sensibilidade. O resultado é suficiente para aprovar o modelo?

**Resposta:** 
- **Total:** 80 + 5 + 10 + 5 = 100 amostras.
- **Acurácia:** (TP + TN) / Total = (5 + 80) / 100 = **85%**
- **Sensibilidade (Recall):** TP / (TP + FN) = 5 / (5 + 10) = 5 / 15 = **33,3%**
**Conclusão:** Não é suficiente. Apesar da alta acurácia (mascarada pelo grande número de verdadeiros negativos), a sensibilidade de 33,3% significa que o modelo deixa escapar a maioria (66,6%) dos casos reais positivos (aqueles que realmente queremos prever). 

## 7. Valores reais 100, 120, 140 e previsões 110, 115, 130. Calcule MAE e informe a unidade se o alvo for vendas em reais.

**Resposta:** 
- Erros absolutos: |100 - 110| = 10; |120 - 115| = 5; |140 - 130| = 10.
- Soma dos erros = 25.
- **MAE** = 25 / 3 = **8,33**.
- A unidade do MAE é a mesma da variável alvo, logo: **R$ 8,33**.

## 8. Um R² de -0,2 significa “20% de acertos negativos”? Explique.

**Resposta:** Não. R² não é uma porcentagem de acertos, é a proporção da variância dos dados que o modelo explica. Um R² negativo significa que o modelo é pior do que uma linha de base "burra" (como prever sempre a média de todos os valores da base). Ou seja, o modelo tem mais erro quadrático do que simplesmente usar a média real, sendo completamente ineficaz.

## 9. Uma árvore tem acurácia 1,00 no treino e 0,65 na validação. Que hipótese investigar e como?

**Resposta:** A hipótese é **Sobreajuste (Overfitting)**. O modelo decorou os ruídos do treino (tirando 100%) e perdeu poder de generalização, indo mal na validação (65%). Devemos investigar reduzindo a complexidade do modelo (ex: diminuindo a profundidade máxima da árvore ou aumentando o número mínimo de amostras por folha) e verificando se a acurácia de validação melhora.

## 10. Por que escolher a profundidade pelo teste prejudica a estimativa final?

**Resposta:** Se usarmos o conjunto de teste iterativamente para ajustar a profundidade (hiperparâmetro), o conjunto de teste deixa de ser inédito e passa a fazer parte do treinamento. O modelo ficará enviesado para se sair bem naqueles dados específicos. Quando ele enfrentar dados novos e reais de produção, a performance será muito pior do que a estimada.

## 11. Baixa frequência associada à evasão demonstra causa? Proponha explicação alternativa.

**Resposta:** Não, demonstra apenas correlação. Correlação não implica causalidade.
**Explicação alternativa:** O aluno pode ter sofrido uma mudança de turno no trabalho (causa real), o que fez com que ele começasse a faltar às aulas (sintoma 1) e, posteriormente, precisasse abandonar o curso (sintoma 2). 

## 12. Um modelo sinaliza um estudante como possível evasão. Que ação responsável pode ser adotada?

**Resposta:** Deve-se adotar uma intervenção humana, de escuta e acolhimento. Um tutor ou professor pode enviar um convite amistoso para mentoria, oferecer apoio com revisões de matéria ou apoio psicológico/financeiro, sem jamais mencionar que ele foi "marcado por um algoritmo" e sem tomar nenhuma ação automatizada punitiva (como cancelamento da matrícula).

## Fontes adicionais consultadas

**Material das aulas de Ciência de Dados e Aprendizado de Máquina; IA;**

[Voltar ao início](README.md).
