# Previsão de Churn em Serviço de Streaming

Modelo de classificação que estima a probabilidade de um assinante cancelar o serviço, construído no Orange Data Mining com uma base simulada de 1.000 clientes.

Projeto da disciplina Machine Learning & Chatbot, do curso de Análise e Desenvolvimento de Sistemas da UniFECAF.

## Problema

Uma empresa de streaming fictícia vê os cancelamentos crescerem, principalmente entre assinantes novos. O objetivo é identificar com antecedência quem está em risco, para que a equipe de retenção atue antes do cancelamento.

## Dados

`dados/dados_clientes_streaming.csv`: 1.000 clientes simulados (nenhum dado real), com perfil demográfico, consumo, pagamento e relacionamento com o suporte. A variável alvo é `Cancelou` (Sim/Não), o que torna o caso uma classificação binária.

## Metodologia

Fluxo no Orange (`previsao_churn_streaming.ows`):

```
File → Outliers → Preprocess → Logistic Regression / Random Forest → Test & Score → Confusion Matrix / ROC
```

- Remoção de outliers com Local Outlier Factor (20 vizinhos, 10% de contaminação): 90 registros removidos
- Padronização das variáveis numéricas (média 0, desvio-padrão 1)
- Validação cruzada estratificada com 10 folds

## Resultados

| Modelo | AUC | Acurácia | F1 | Precisão | Recall |
| --- | --- | --- | --- | --- | --- |
| Regressão Logística | 0,701 | 0,647 | 0,561 | 0,610 | 0,519 |
| Random Forest | 0,693 | 0,651 | 0,547 | 0,625 | 0,486 |

Os dois modelos tiveram desempenho equivalente. A Regressão Logística foi escolhida por ter AUC ligeiramente maior e coeficientes fáceis de explicar para a área de negócio.

**Fatores com maior peso no cancelamento** (Information Gain, Gain Ratio e Gini): tempo como cliente, horas de uso mensal, consumo de conteúdo original, recebimento de desconto promocional e número de chamados ao suporte.

## Recomendações de negócio

- Reforçar o onboarding nos primeiros meses, período de maior risco
- Usar a probabilidade prevista para segmentar clientes em faixas de risco e direcionar descontos só à faixa alta
- Incentivar a troca do boleto por pagamento recorrente
- Suporte proativo para clientes com muitos chamados

## Como reproduzir

1. Instale o [Orange Data Mining](https://orangedatamining.com/download/).
2. Abra `previsao_churn_streaming.ows`.
3. No widget **File**, aponte para `dados/dados_clientes_streaming.csv`.

## Próximos passos

- Reimplementar o fluxo em Python (pandas e scikit-learn)
- Tratar o desbalanceamento de classes e ajustar o limiar de decisão para priorizar recall

## Autor

Guilherme Oliveira · [LinkedIn](https://www.linkedin.com/in/guilhermeoss)
