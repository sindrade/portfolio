# SQL

## 🍜 Estudo de caso - Danny's Diner

<img src="https://github.com/sindrade/portfolio/assets/24964847/1cab1f74-ef55-488f-880b-2ad729237afc" width="400" height="400" />

## Índice
* [Declaração do Problema](#declaração-do-problema)
* [Conjunto de dados](#conjunto-de-dados)
* [Perguntas do estudo de caso](#-perguntas-do-estudo-de-caso)
<!-- * soluções
* Limitações -->
<br>

## Declaração do problema
Danny quer usar os dados para responder a algumas perguntas simples sobre seus clientes, especialmente sobre seus padrões de visita, quanto dinheiro gastaram e também quais itens de menu são seus favoritos. Ter essa conexão mais profunda com seus clientes o ajudará a oferecer uma experiência melhor e mais personalizada para seus clientes fiéis.

<br>

## Ferramentas utilizadas
SQL Server
<br>

## Conjunto de dados
O conjunto de dados do projeto foi baixado deste [site](https://www.db-fiddle.com/f/2rM8RAnq7h5LLDTzZiRWcd/138) [LINK EXTERNO]
<br><br>
<hr>

## 👜 Perguntas do estudo de caso
### 1. Qual o valor total que cada cliente gastou no restaurante?
```sql
SELECT
  s.customer_id AS [ID Cliente],
  SUM(m.price) AS [Total em compras]
FROM
  sales s
  INNER JOIN menu m ON s.product_id = m.product_id
GROUP BY
  s.customer_id;
```

| ID Cliente | Vendas Totais |
| ---------- | ------------- |
| A | 76 |
| B | 74 |
| C | 36 |

**Resposta:** Os clientes A, B e C gastaram US$ 76, US$ 74 e US$ 36 respectivamente.

<hr>

### 2. Quantas vezes cada cliente visitou o restaurante?
```sql
SELECT 
  customer_id AS [ID Cliente], 
  COUNT(DISTINCT order_date) AS [No. de Dias]
FROM 
  sales
GROUP BY customer_id;
```
**Resposta:** 
| ID Cliente | No. de Visitas |
| ---------- | ---------- |
| A | 4 |
| B | 6 |
| C | 2 |

* Cliente A vistou 4 vezes.
* Cliente B visitou 6 vezes.
* Cliente C visitou 2 vezes.

<hr>

### 3. Qual foi o primeiro item do cardápio adquirido por cada cliente?

```sql
WITH Ordered_Sales AS (
  SELECT 
    s.customer_id, 
    s.order_date, 
    m.product_name,
    DENSE_RANK() OVER (
      PARTITION BY s.customer_id 
      ORDER BY s.order_date) AS purchase_rank
  FROM 
    sales s
  INNER JOIN 
    menu m ON s.product_id = m.product_id
)
SELECT 
  customer_id AS [ID do Cliente], 
  product_name AS [Nome do Produto]
FROM 
  Ordered_Sales
WHERE 
  purchase_rank = 1
GROUP BY 
  customer_id, product_name;
```  

**Resultado:**
| ID do Cliente | Nome do Produto |
| --- | --- |
| A | curry |
| A | sushi | 
| B | curry |
| C | ramen | 

O cliente ‘A’ fez dois pedidos em sua primeira compra, incluindo curry e sushi, o cliente’ B’ comprou curry e o cliente ‘C’ ramen.

<hr>
