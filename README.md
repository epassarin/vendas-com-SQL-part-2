# vendas-com-SQL-part-2
Arquivo para copiar
TODA A ATIVIDADE COMPLETAMENTE REALIZADA** com **avaliação prática** ou pode ser usado **uso como material de estudo**.


***

# Banco de Dados – SQL

## Analisando dados de vendas com SQL (Parte 2)

### Roteiro de Atividade Prática – **Solução Completa**
**Tempo proposto:** 45 minutos

***
## 1. Objetivo da Atividade

O objetivo desta atividade é **analisar dados de vendas de um e‑commerce utilizando SQL**, aplicando:

*   cláusula `WHERE` para filtros temporais;
*   funções de agregação (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`);
*   `GROUP BY` para agrupamentos;
*   `JOIN` para relacionar tabelas;
*   subconsultas para análises mais elaboradas.

Essa atividade simula relatórios reais utilizados em **áreas de negócio, BI e back‑end**.

***

## 2. Preparação do Ambiente

### 2.1 IDE / Programa recomendado

Você pode utilizar **qualquer SGBD relacional**, sendo os mais indicados:

✅ **iremos usar o SQLite**
✅ **Primeiro crie uma nova pasta - pode ser** `/venda-com-sql` **onde voce voce constuma criar os projetos das nossas aulas*
✅ **entre no VSCode, abra a pasta que você criou e crie um nova Query**
✅ **Para criar uma nova QUERY pressione** `CTRL` **+** `SHIF` **+** `P` **e selecione SQLite NEW QUERY** 



## 3. Estrutura do Banco de Dados

#### Digite o texto abaixo nessa nova QUERY ou simplesmente copie e cole para que possamos fazer os devidos testes.

### 3.1 Tabela produtos

```sql
CREATE TABLE produtos (
    id_produto INT PRIMARY KEY,
    nome VARCHAR(100),
    categoria VARCHAR(50),
    preco DECIMAL(10,2)
);
```

***

### 3.2 Tabela vendas

```sql
CREATE TABLE vendas (
    id_venda INT PRIMARY KEY,
    id_produto INT,
    quantidade INT,
    data_venda DATE,
    FOREIGN KEY (id_produto) REFERENCES produtos(id_produto)
);
```

***

## 4. Contexto da Análise

Você precisa analisar **as vendas dos últimos três meses** para responder perguntas estratégicas como:

*   produto mais vendido;
*   faturamento por categoria;
*   média de itens vendidos;
*   maiores e menores valores;
*   categoria mais vendida.

***

## 5. Pergunta 1 – Produto mais vendido nos últimos 3 meses ✅ (já fornecida)

*(mantida como referência oficial)*

```sql
SELECT p.nome, COUNT(v.id_venda) AS total_vendas
FROM produtos p
INNER JOIN vendas v ON p.id_produto = v.id_produto
WHERE v.data_venda >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH)
GROUP BY p.nome
ORDER BY total_vendas DESC
LIMIT 1;
```

***

## 6. Pergunta 2 – Valor total de vendas por categoria no mês passado

### Consulta SQL

```sql
SELECT p.categoria,
       SUM(v.quantidade * p.preco) AS total_vendas
FROM produtos p
INNER JOIN vendas v ON p.id_produto = v.id_produto
WHERE MONTH(v.data_venda) = MONTH(DATE_SUB(CURDATE(), INTERVAL 1 MONTH))
  AND YEAR(v.data_venda) = YEAR(DATE_SUB(CURDATE(), INTERVAL 1 MONTH))
GROUP BY p.categoria;
```

### Explicação

*   `SUM(v.quantidade * p.preco)` → calcula o faturamento;
*   filtro por mês anterior;
*   `GROUP BY p.categoria` → total por categoria.

***

## 7. Pergunta 3 – Média de produtos vendidos por pedido (últimos 3 meses)

### Consulta SQL

```sql
SELECT AVG(v.quantidade) AS media_produtos_por_pedido
FROM vendas v
WHERE v.data_venda >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH);
```

### Explicação

*   `AVG(quantidade)` → média de itens por venda;
*   filtro de três meses.

***

## 8. Pergunta 4 – Maior e menor valor de venda (últimos 3 meses)

### Consulta SQL

```sql
SELECT 
    MAX(v.quantidade * p.preco) AS maior_venda,
    MIN(v.quantidade * p.preco) AS menor_venda
FROM vendas v
INNER JOIN produtos p ON v.id_produto = p.id_produto
WHERE v.data_venda >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH);
```

### Explicação

*   calcula valor total de cada venda;
*   `MAX` e `MIN` identificam extremos.

***

## 9. Pergunta 5 – Categoria com maior quantidade de produtos vendidos

### Consulta SQL

```sql
SELECT p.categoria,
       SUM(v.quantidade) AS total_itens_vendidos
FROM produtos p
INNER JOIN vendas v ON p.id_produto = v.id_produto
WHERE v.data_venda >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH)
GROUP BY p.categoria
ORDER BY total_itens_vendidos DESC
LIMIT 1;
```

### Explicação

*   soma quantidades vendidas;
*   ordena da maior para a menor.

***

## 10. 🔥 Desafio Adicional – Faturamento mensal por categoria (últimos 3 meses)

### Consulta SQL (USANDO SUBCONSULTA)

```sql
SELECT 
    p.categoria,
    YEAR(v.data_venda) AS ano,
    MONTH(v.data_venda) AS mes,
    SUM(v.quantidade * p.preco) AS faturamento
FROM produtos p
INNER JOIN vendas v ON p.id_produto = v.id_produto
WHERE v.data_venda >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH)
GROUP BY p.categoria, ano, mes
ORDER BY ano, mes, faturamento DESC;
```

### Explicação

*   agrupa por mês e categoria;
*   calcula faturamento mensal;
*   ideal para relatórios financeiros.

***

## 11. Resumo dos Conceitos Aplicados

| Conceito    | Onde foi usado               |
| ----------- | ---------------------------- |
| WHERE       | Filtro por período           |
| JOIN        | Relacionar produtos e vendas |
| GROUP BY    | Soma por categoria/mês       |
| SUM         | Faturamento                  |
| COUNT       | Quantidade de vendas         |
| AVG         | Média                        |
| MIN / MAX   | Extremos                     |
| Subconsulta | Análise avançada             |

***

## 12. Forma de Entrega da Atividade

📌 **Entrega recomendada:**

*   Documento **PDF ou DOCX** contendo:
    *   contextualização;
    *   todas as consultas SQL;
    *   explicação de cada query;
*   Código SQL organizado;
*   Nome e turma preenchidos;
*   Clareza e organização serão critérios de avaliação.

***

## 13. Conclusão

Esta atividade demonstra como o SQL é uma ferramenta essencial para **análise de dados reais**, permitindo extrair informações estratégicas para tomada de decisão. O uso correto de `JOIN`, funções de agregação e filtros temporais torna possível transformar dados brutos em conhecimento de negócio.


