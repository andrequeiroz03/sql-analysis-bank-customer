# Bank Customer Analytics — SQL

Análises de negócio sobre a base de clientes de um banco, em SQL no Databricks

## Contexto
Um time de negócio pede análises sobre concentração de saldo, clientes premium, maturidade da base e segmentação para campanhas.

## Dataset
- Fonte: https://www.kaggle.com/datasets/srisyra02/bank-customer-churn-prediction
- ~10 mil clientes, com colunas como Geography, Age, Tenure, Balance, NumOfProducts
- Tabela utilizada: `bank_customer_churn`

## Análises

### 1. Concentração do saldo na carteira
- **Pedido:** entender quanto do saldo do banco depende de poucos clientes. Considerando só os clientes com saldo positivo, dividir a base em 10 grupos iguais, do maior saldo para o menor. Para cada grupo, mostrar:
  - quantos clientes tem;
  - o saldo total;
  - quanto isso representa do saldo total do banco, em %;
  - o percentual acumulado até aquele grupo;
  - o saldo médio.

  Uma linha por grupo, ordenada do maior para o menor saldo.
- **Técnicas:** `ntile`, soma acumulada com window function, CTEs encadeadas

![](result_images/pedido_01.png)

### 2. Clientes premium por país
- **Pedido:** uma lista dos 10 clientes com maior saldo em cada país, para a equipe comercial abordar pessoalmente. Para cada cliente, mostrar:
  - ID, país, idade e saldo;
  - a posição dele no ranking do próprio país;
  - quantas vezes o saldo dele é maior que o saldo médio dos clientes daquele país (considerando só quem tem saldo positivo);
  - a diferença de saldo para o cliente logo abaixo dele no ranking.

  Se houver empate de saldo, os empatados aparecem com a mesma posição. Organizar por país e depois por posição.
- **Técnicas:** `dense_rank`, `lead`, `PARTITION BY`, média condicional

![](result_images/pedido_02.png)

### 3. Maturidade por tempo de relacionamento
- **Pedido:** saber se clientes mais antigos são mais engajados com o banco. Agrupar os clientes por anos de relacionamento (0 a 10), cada ano como um grupo. Para cada grupo, mostrar:
  - o número de clientes;
  - o saldo médio (só clientes com saldo positivo);
  - o % de clientes com saldo zero;
  - o % de clientes com 2 ou mais produtos.

  Para o saldo médio e o % com 2 ou mais produtos, mostrar também a variação em relação ao ano anterior de relacionamento, em pontos absolutos. Uma linha por ano, em ordem crescente.
- **Técnicas:** `lag`, agregações condicionais

![](result_images/pedido_03.png)

### 4. Segmentação por faixa etária e país
- **Pedido:** uma visão para decidir onde investir em campanhas. Criar as faixas etárias 18 a 29, 30 a 39, 40 a 49, 50 a 59 e 60 ou mais, e cruzar cada faixa com cada país. Para cada combinação de faixa e país, mostrar:
  - o número de clientes e quanto isso representa da base total, em %;
  - o saldo médio entre os clientes com saldo positivo;
  - o % de clientes com saldo zero;
  - um índice de saldo médio em que 100 é a média geral do banco (acima de 100 está acima da média).
- **Técnicas:** `CASE`, índice de saldo médio (base 100), `COUNT() OVER()`

![](result_images/pedido_04.png)