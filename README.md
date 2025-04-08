---
description: note about Spark SQL module
---

# SQL

SQL Process

Spark中,sql模块允许用户通过简单的SQL语句进行数据运算, spark会将这些查询sql转换为RDD DAG去执行. 这个过程包含了SQL Parse, SQL Analysis, SQL Optimize, Physical Plan Codegen等过程.

<figure><img src=".gitbook/assets/mermaid-202543 185119.png" alt=""><figcaption></figcaption></figure>

### SQL Parser

在理解Spark如何解析SQL前, 有必要对ANTLR(Another Tool for Language Recongnition)有一个基础的认识. Spark实现了自由的Antlr 词法和语法文件来支持对SQL文本的理解. Antlr会根据词法, 语法文件将SQL文本转换为一颗Abstract syntax tree(AST).

接下来看看Spark如何解析SQL\~\~

#### Prepare Antlr Files

我们可以根据以下链接获取Spark实现的词法文件, 语法文件:

1. LexerFile: [https://github.com/apache/spark/blob/master/sql/api/src/main/antlr4/org/apache/spark/sql/catalyst/parser/SqlBaseLexer.g4](https://github.com/apache/spark/blob/master/sql/api/src/main/antlr4/org/apache/spark/sql/catalyst/parser/SqlBaseLexer.g4)
2. GrammarFile: [https://github.com/apache/spark/blob/master/sql/api/src/main/antlr4/org/apache/spark/sql/catalyst/parser/SqlBaseParser.g4](https://github.com/apache/spark/blob/master/sql/api/src/main/antlr4/org/apache/spark/sql/catalyst/parser/SqlBaseParser.g4)

ANTLR文件准备好后, 需要使用ANTLR工具或插件将文件转换为对应的类, 可以从 [ANTLR website](https://www.antlr.org/)中去根据指南进行生成. 在相关类文件生成完成后, ANTLR提供了两种SQL解析模式, 分别是Listener与Visitor. Spark是通过Visitor模式去对SQL内容进行解析的.

我们可以在Spark的DataTypeAstBuilder与AstBuilder中发现其对Visitor模式的解析支持.

<figure><img src=".gitbook/assets/截屏2025-04-07 02.11.39.png" alt=""><figcaption></figcaption></figure>

在分析Spark的SQL Parse之前, 需要对Spark的转换类有一个基础了解



**Parse The SQL**

When Spark parses SQL, the process involves transforming the ANTLR abstract syntax tree (AST) using the `AstBuilder`. This process resolves the tree from an unresolved state into a resolved logical plan. Spark achieves this by implementing a base class called `TreeNode`, which forms the foundation for handling and transforming the tree structure systematically. This approach facilitates efficient processing and optimization of SQL queries.

AstBuilder processes the SQL AST by mapping different components to Spark's internal representations, such as expressions and logical plans. Each node type in the tree is translated using specific methods aimed at accurately reflecting its logical structure within Spark's execution framework.&#x20;



The `AbstractSqlParser` in Spark plays a crucial role in parsing SQL text into a `LogicalPlan`. When a SQL query is executed using `spark.sql`, it first reaches the parser stage, where the `astBuilder` is employed to translate the SQL text into a logical plan. This process is pivotal for transforming the raw SQL syntax into a structured format that Spark can optimize and execute efficiently. The `parsePlan` method exemplifies this by invoking `astBuilder.visitSingleStatement` to handle specific SQL structures and ensure they are correctly interpreted as logical plans. If the parsing process encounters an unsupported SQL statement, it throws a `sqlStatementUnsupportedError`. This mechanism ensures that only valid and optimizable SQL queries are processed further in Spark's query execution pipeline.

```
  override def parsePlan(sqlText: String): LogicalPlan = parse(sqlText) { parser =>
    val ctx = parser.singleStatement()
    withOrigin(ctx, Some(sqlText)) {
      astBuilder.visitSingleStatement(ctx) match {
        case plan: LogicalPlan => plan
        case _ =>
          val position = Origin(None, None)
          throw QueryParsingErrors.sqlStatementUnsupportedError(sqlText, position)
      }
    }
  }
```

Talk is cheap, let's see the real SQL parse

```
SELECT A.*
FROM TABLE_A A
LEFT JOIN TABLE_B B
ON A.ID=B.ID
```

<figure><img src=".gitbook/assets/截屏2025-04-07 02.58.55.png" alt=""><figcaption></figcaption></figure>

The above picture is from anltr tool, which parse the sql text, then get an ast tree.



### Logical Plan Analysis

### Logical Plan Optimizer

### Physical Plan

### Codegen

## Native engine——SIMD

### SIMD About Database Paper Note

### Native engine component

#### Gluten

#### Blaze





















