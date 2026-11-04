# Unidade III - Gestão de Projetos em Ciências de Dados

Esta pasta contém o documento de definição do projeto **"Reembolso em Dados"**: a operadora de saúde de autogestão estuda os próprios dados de reembolso assistencial para gerar ideias e subsidiar a tomada de decisão.

## Arquivos

| Arquivo | Descrição |
|---|---|
| `Template-Definicao-Projeto-Ciencia-de-Dados.md` | Template completo (seções 1 a 16 e Anexo A), versão V03. |
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
6. **Anexo A.** Referência técnica da consulta e seis ajustes obrigatórios para a versão de análise.

## Pontos a validar

- Os limites de 11 registros e de 98% de correspondência de chaves são sugestões, a serem confirmados com o DPO.
- A base legal do tratamento deve ser definida e documentada pela operadora.
- Os campos de aprovação, datas e checklists estão em branco para preenchimento pela equipe.
- A consulta original **não foi alterada**. O Anexo A lista as mudanças necessárias para a versão pseudonimizada.
