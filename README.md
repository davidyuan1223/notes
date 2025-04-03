---
description: note about Spark SQL module
---

# SQL

SQL Process

In Spark, the SQL module allows users to execute data computation using simple SQL queries. Spark translates these SQL queries into an RDD DAG (Directed Acyclic Graph) for execution. This process involves various stages such as parsing the SQL, creating a logical plan, optimizing it, and finally generating the physical execution plan for efficient data processing on Spark's distributed data processing engine.

<figure><img src=".gitbook/assets/mermaid-202543 185119.png" alt=""><figcaption></figcaption></figure>

### SQL Parser

Before understanding how Spark parses SQL text, it's essential to have a basic knowledge of ANTLR (Another Tool for Language Recognition). Spark implements its own ANTLR lexer and grammar files to support the understanding of SQL content. ANTLR is used to transform the SQL text into an abstract syntax tree (AST), serving as the foundation for subsequent stages of query processing.

Let's see Spark how to parse the SQL\~\~

#### Prepare Antlr Files

We could found the files in :&#x20;

1. LexerFile: [https://github.com/apache/spark/blob/master/sql/api/src/main/antlr4/org/apache/spark/sql/catalyst/parser/SqlBaseLexer.g4](https://github.com/apache/spark/blob/master/sql/api/src/main/antlr4/org/apache/spark/sql/catalyst/parser/SqlBaseLexer.g4)
2. GrammarFile: [https://github.com/apache/spark/blob/master/sql/api/src/main/antlr4/org/apache/spark/sql/catalyst/parser/SqlBaseParser.g4](https://github.com/apache/spark/blob/master/sql/api/src/main/antlr4/org/apache/spark/sql/catalyst/parser/SqlBaseParser.g4)





#### Implement Anltr Visitor Or Listener

#### Parse The SQL



### Logical Plan Analysis

### Logical Plan Optimizer

### Physical Plan

### Codegen

## Native engine——SIMD

### SIMD About Database Paper Note

### Native engine component

#### Gluten

#### Blaze





















