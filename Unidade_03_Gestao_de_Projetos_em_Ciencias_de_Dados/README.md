# Unidade III - Gestão de Projetos em Ciências de Dados

Esta pasta contém o documento de definição do projeto **"Reembolso em Dados"**: a operadora de saúde de autogestão estuda os próprios dados de reembolso assistencial para descobrir **onde agir** (estados, municípios, procedimentos, especialidades e prestadores), reduzir o reembolso, tornar constante o uso da rede credenciada e diminuir o custo assistencial da medida reparadora prevista na RN 566/2022 da ANS.

## Arquivos

| Arquivo | Descrição |
|---|---|
| `Template-Definicao-Projeto-Ciencia-de-Dados.md` | Template completo (seções 1 a 16 e Anexo A), versão V04. |
| `README.md` | Este resumo do que foi feito. |

## Histórico do que foi feito

1. **Substituição do tema original.** O template antigo ("Helping Hands", plataforma de doações) foi totalmente substituído por um projeto de ciência de dados sobre reembolso assistencial.
2. **Leitura da consulta SQL (Trino/Presto)** que extrai os itens pagos de reembolso. Ela tem três CTEs e um resultado final:
   - `BaseTratamento`: lê a tabela fato e converte `DTAFINALIZACAO` para texto uma vez.
   - `BaseDataTratada`: converte para `DATE`, tratando nulos, valores-padrão (`0`, `19000101`) e formatos `AAAAMMDD` e `AAAA-MM-DD`.
   - `BaseReembolso`: `LEFT JOIN` com seis dimensões.
   - Resultado: `SELECT DISTINCT` no nível de item pago, com `Guias`, `Valor_pagos` e `Data_finalizacao`.
3. **Primeira versão (V02).** Projeto de ML voltado à previsão de prazo, risco de atraso e anomalias.
4. **Reenquadramento (V03).** O projeto passou a ter a operadora como dona e analista dos próprios dados. O foco é exploração, segmentação e geração de hipóteses e ideias, com modelos como apoio e revisão humana. Mudanças principais:
   - novo resumo, problema central, partes interessadas, objetivos e perguntas de negócio (incluindo segmentação e matriz impacto × esforço);
   - novas hipóteses (H4 sobre perfis de solicitação);
   - novo entregável "Banco de ideias e hipóteses" e novo critério de sucesso "Geração de ideias";
   - plano de trabalho com etapa própria de exploração e ideação;
   - uso apenas de dados internos, sem fontes externas obrigatórias.
5. **Adequação à LGPD.**
   - O documento não contém dados reais.
   - Há tabela de classificação por coluna e tratamento (excluir, pseudonimizar com HMAC, generalizar, agregar).
   - Regras: supressão de grupos com menos de 11 registros, `CID` apenas por grupo, datas em mês/ano, ambiente interno, revisão humana (art. 20), ROPA, RIPD e RLS no Power BI.
6. **Anexo A.** Referência técnica da consulta e ajustes obrigatórios para a versão de análise.
7. **Equipe.** Eduardo Salgueiro foi substituído por Júlio César, que passou a ser o único integrante (papéis consolidados).
8. **Foco em rede e custo (V04).** Com a síntese do projeto, o documento foi refeito em torno de reduzir o reembolso e aumentar o uso da rede credenciada:
   - resumo, problema, partes interessadas, objetivos, perguntas, hipóteses, entregáveis, critérios de sucesso, plano e riscos reescritos para o diagnóstico por UF, município, procedimento, especialidade e prestador;
   - novos conceitos: **lacuna de rede**, **subutilização da rede**, **custo evitável** e classificação do reembolso (medida reparadora, livre escolha e outros);
   - novas fontes necessárias (a obter): cadastro da rede credenciada, custo e utilização na rede e beneficiários agregados por município. Sem elas, só é possível descrever o reembolso;
   - novas partes interessadas: rede/credenciamento, regulação/jurídico e financeiro/atuária;
   - LGPD: regra para análise de prestadores (pseudonimizados, com reidentificação autorizada pelo DPO/TI), municípios pequenos agrupados e resultados sempre agregados;
   - salvaguardas éticas: o projeto não visa dificultar ou negar reembolsos devidos, e os rankings de prestadores não têm finalidade punitiva;
   - Anexo A com três novos ajustes (padronização de município e prestador, classificação do reembolso e integração com rede e custo).

## Pontos a validar

- Os limites de 11 registros e de 98% de correspondência de chaves são sugestões, a serem confirmados com o DPO.
- A base legal do tratamento deve ser definida e documentada pela operadora.
- A descrição da RN 566/2022 e o mapeamento entre tipo/motivo de reembolso e medida reparadora devem ser confirmados pela regulação e pelo jurídico da operadora.
- As bases de rede credenciada, custo na rede e beneficiários agregados precisam ser solicitadas às áreas donas.
- Os campos de aprovação, datas e checklists estão em branco para preenchimento pela equipe.
- A consulta original **não foi alterada**. O Anexo A lista as mudanças necessárias para a versão pseudonimizada.
