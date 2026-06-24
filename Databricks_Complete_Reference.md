---
layout: default
title: Databricks Complete Reference
---

# Databricks Complete Reference for Data Analysts & Engineers

A comprehensive reference for working with Python (PySpark) in Databricks — from reading data and basic exploration through Delta Lake, ETL pipelines, orchestration, performance tuning, and production patterns.

Every section explains the concept, shows the syntax, and includes practical examples you can paste directly into a Databricks notebook.

---

## Table of Contents

### Part 1: Getting Started
1. [Databricks Notebook Basics](#1-databricks-notebook-basics)
2. [The SparkSession and Databricks Utilities](#2-the-sparksession-and-databricks-utilities)
3. [Magic Commands](#3-magic-commands)
4. [Widgets — Parameterized Notebooks](#4-widgets--parameterized-notebooks)

### Part 2: Reading and Writing Data
5. [Reading Data — All Sources](#5-reading-data--all-sources)
6. [Viewing and Inspecting Data](#6-viewing-and-inspecting-data)
7. [Writing Data — All Destinations](#7-writing-data--all-destinations)
8. [Working with Unity Catalog and Schemas](#8-working-with-unity-catalog-and-schemas)

### Part 3: DataFrame Operations
9. [Selecting Columns](#9-selecting-columns)
10. [Filtering Rows](#10-filtering-rows)
11. [Adding and Computing New Columns](#11-adding-and-computing-new-columns)
12. [Renaming Columns](#12-renaming-columns)
13. [Dropping Columns and Rows](#13-dropping-columns-and-rows)
14. [Data Types and Casting](#14-data-types-and-casting)
15. [Handling Null and Missing Values](#15-handling-null-and-missing-values)
16. [String Functions](#16-string-functions)
17. [Date and Timestamp Functions](#17-date-and-timestamp-functions)
18. [Sorting and Ordering](#18-sorting-and-ordering)
19. [Grouping and Aggregation](#19-grouping-and-aggregation)
20. [Joins](#20-joins)
21. [Unions and Set Operations](#21-unions-and-set-operations)
22. [Window Functions](#22-window-functions)
23. [Pivot and Unpivot](#23-pivot-and-unpivot)
24. [Deduplication](#24-deduplication)
25. [Conditional Logic — WHEN, OTHERWISE, COALESCE](#25-conditional-logic--when-otherwise-coalesce)
26. [Working with Arrays and Structs](#26-working-with-arrays-and-structs)
27. [Working with JSON Columns](#27-working-with-json-columns)
28. [User Defined Functions (UDFs)](#28-user-defined-functions-udfs)

### Part 4: SQL in Databricks
29. [Running SQL in Notebooks](#29-running-sql-in-notebooks)
30. [Mixing SQL and PySpark](#30-mixing-sql-and-pyspark)

### Part 5: Delta Lake
31. [What is Delta Lake](#31-what-is-delta-lake)
32. [Creating and Managing Delta Tables](#32-creating-and-managing-delta-tables)
33. [MERGE — Upserts and SCD](#33-merge--upserts-and-scd)
34. [Time Travel and Versioning](#34-time-travel-and-versioning)
35. [OPTIMIZE, VACUUM, and Maintenance](#35-optimize-vacuum-and-maintenance)
36. [Schema Evolution and Enforcement](#36-schema-evolution-and-enforcement)
37. [Change Data Feed (CDF)](#37-change-data-feed-cdf)

### Part 6: Data Engineering Patterns
38. [Bronze-Silver-Gold (Medallion Architecture)](#38-bronze-silver-gold-medallion-architecture)
39. [Incremental Data Loading](#39-incremental-data-loading)
40. [Auto Loader — Streaming File Ingestion](#40-auto-loader--streaming-file-ingestion)
41. [Structured Streaming](#41-structured-streaming)
42. [Delta Live Tables (DLT)](#42-delta-live-tables-dlt)
43. [Orchestration with Databricks Workflows](#43-orchestration-with-databricks-workflows)

### Part 7: Performance and Best Practices
44. [Partitioning Strategies](#44-partitioning-strategies)
45. [Z-Ordering and Liquid Clustering](#45-z-ordering-and-liquid-clustering)
46. [Caching and Persistence](#46-caching-and-persistence)
47. [Broadcast Joins and Skew Handling](#47-broadcast-joins-and-skew-handling)
48. [Explain Plans and Debugging](#48-explain-plans-and-debugging)
49. [Common Errors and Fixes](#49-common-errors-and-fixes)

### Part 8: Utilities and Productivity
50. [File System Operations (dbutils.fs)](#50-file-system-operations-dbutilsfs)
51. [Secrets Management](#51-secrets-management)
52. [Connecting to External Systems](#52-connecting-to-external-systems)
53. [Pandas on Spark (pandas API)](#53-pandas-on-spark-pandas-api)
54. [Visualization in Notebooks](#54-visualization-in-notebooks)
55. [Common Recipes and Patterns](#55-common-recipes-and-patterns)

---

# Part 1: Getting Started

---

## 1. Databricks Notebook Basics

### Cell Types

Databricks notebooks support multiple languages in a single notebook. The default language is set at notebook level (Python, SQL, Scala, or R).

```
# Python cell (default if notebook language is Python)
print("Hello Databricks")

# to use a different language in a cell, use a magic command at the top:
```

```sql
-- %sql
SELECT * FROM my_table LIMIT 10
```

### Keyboard Shortcuts

| Action | Shortcut |
|--------|----------|
| Run cell | `Shift + Enter` |
| Run cell, stay in place | `Ctrl + Enter` |
| Add cell above | `A` (in command mode) |
| Add cell below | `B` (in command mode) |
| Delete cell | `D, D` (press D twice) |
| Undo delete | `Z` |
| Toggle comment | `Ctrl + /` |
| Find and replace | `Ctrl + H` |
| Autocomplete | `Tab` |
| View documentation | `Shift + Tab` |

### Notebook-Scoped Libraries

```python
# install libraries for this notebook only
%pip install faker openpyxl xlsxwriter

# restart Python after installing
dbutils.library.restartPython()
```

---

## 2. The SparkSession and Databricks Utilities

### SparkSession

In Databricks, `spark` is pre-configured and available in every notebook.

```python
# already available — no need to create
spark
# <pyspark.sql.session.SparkSession>

# check Spark version
spark.version

# check configuration
spark.conf.get("spark.sql.shuffle.partitions")

# set configuration
spark.conf.set("spark.sql.shuffle.partitions", "200")
spark.conf.set("spark.sql.adaptive.enabled", "true")
```

### dbutils — Databricks Utilities

```python
# see all available utilities
dbutils.help()

# specific utility help
dbutils.fs.help()
dbutils.widgets.help()
dbutils.secrets.help()
dbutils.notebook.help()
```

### Running Other Notebooks

```python
# run another notebook (passes execution context)
dbutils.notebook.run("/Users/you@company.com/setup_notebook", timeout_seconds=300)

# with parameters
result = dbutils.notebook.run(
    "/Users/you@company.com/process_data",
    timeout_seconds=600,
    arguments={"date": "2024-03-15", "env": "prod"}
)
print(f"Notebook returned: {result}")

# exit a notebook with a return value
dbutils.notebook.exit("SUCCESS")
```

---

## 3. Magic Commands

```python
# %python — run Python (default in Python notebooks)
%python
print("Python code")

# %sql — run SQL
%sql
SELECT current_date(), current_timestamp()

# %scala — run Scala
%scala
println("Scala code")

# %r — run R
%r
print("R code")

# %md — render Markdown (for documentation cells)
%md
# This is a Heading
This cell renders as **formatted text**.

# %sh — run shell commands
%sh
ls -la /dbfs/mnt/
pwd
pip list | grep pandas

# %run — execute another notebook inline (shares variables)
%run ./includes/common_functions

# %fs — shortcut for dbutils.fs
%fs ls /mnt/datalake/raw/

# %pip — install Python packages
%pip install great-expectations
```

---

## 4. Widgets — Parameterized Notebooks

Widgets let you build parameterized, reusable notebooks.

```python
# --- create widgets ---

# text input
dbutils.widgets.text("start_date", "2024-01-01", "Start Date")

# dropdown
dbutils.widgets.dropdown("environment", "dev", ["dev", "staging", "prod"], "Environment")

# combobox (dropdown + free text)
dbutils.widgets.combobox("table_name", "sales", ["sales", "orders", "customers"], "Table")

# multiselect
dbutils.widgets.multiselect("departments", "ALL", ["ALL", "Engineering", "Sales", "HR"], "Departments")

# --- get widget values ---
start_date = dbutils.widgets.get("start_date")
env = dbutils.widgets.get("environment")

# use in code
df = spark.read.table(f"{env}_catalog.schema.{table_name}")
df = df.filter(F.col("date") >= start_date)

# --- remove widgets ---
dbutils.widgets.remove("start_date")
dbutils.widgets.removeAll()
```

### Using Widgets in SQL

```sql
-- reference widget values with ${widget_name}
SELECT * FROM sales
WHERE order_date >= '${start_date}'
  AND department IN (${departments})
```

---

# Part 2: Reading and Writing Data

---

## 5. Reading Data — All Sources

### CSV

```python
# basic read
df = spark.read.csv("/mnt/datalake/raw/sales.csv", header=True, inferSchema=True)

# with full options
df = spark.read.format("csv") \
    .option("header", "true") \
    .option("inferSchema", "true") \
    .option("delimiter", ",") \
    .option("quote", '"') \
    .option("escape", '"') \
    .option("multiLine", "true") \
    .option("encoding", "UTF-8") \
    .option("nullValue", "NA") \
    .option("emptyValue", "") \
    .option("dateFormat", "yyyy-MM-dd") \
    .option("timestampFormat", "yyyy-MM-dd HH:mm:ss") \
    .load("/mnt/datalake/raw/sales.csv")

# with explicit schema (faster, safer than inferSchema)
from pyspark.sql.types import StructType, StructField, StringType, IntegerType, DoubleType, DateType

schema = StructType([
    StructField("id", IntegerType(), nullable=False),
    StructField("name", StringType(), nullable=True),
    StructField("amount", DoubleType(), nullable=True),
    StructField("order_date", DateType(), nullable=True),
])
df = spark.read.csv("/mnt/datalake/raw/sales.csv", header=True, schema=schema)

# read multiple CSV files
df = spark.read.csv("/mnt/datalake/raw/sales_*.csv", header=True, inferSchema=True)
df = spark.read.csv(["/path/file1.csv", "/path/file2.csv"], header=True, inferSchema=True)

# read all CSVs in a directory
df = spark.read.csv("/mnt/datalake/raw/sales/", header=True, inferSchema=True)
```

### Parquet

```python
df = spark.read.parquet("/mnt/datalake/processed/sales.parquet")

# partitioned data
df = spark.read.parquet("/mnt/datalake/processed/sales/")
# automatically reads year=2024/month=03/ partitions

# select specific partitions
df = spark.read.parquet("/mnt/datalake/processed/sales/year=2024/")
```

### JSON

```python
# standard JSON (array of objects or one object per line)
df = spark.read.json("/mnt/datalake/raw/events.json")

# multi-line JSON
df = spark.read.json("/mnt/datalake/raw/events.json", multiLine=True)

# with schema
df = spark.read.json("/path/to/data.json", schema=my_schema)
```

### Delta Lake

```python
# read Delta table by path
df = spark.read.format("delta").load("/mnt/datalake/delta/sales")

# read Delta table from catalog (preferred)
df = spark.read.table("catalog.schema.sales")
df = spark.table("catalog.schema.sales")    # shorthand
```

### Excel

```python
# requires com.crealytics:spark-excel library
%pip install openpyxl

# using pandas then converting
import pandas as pd
pdf = pd.read_excel("/dbfs/mnt/datalake/raw/report.xlsx", sheet_name="Sheet1")
df = spark.createDataFrame(pdf)

# if spark-excel is installed on the cluster
df = spark.read.format("com.crealytics.spark.excel") \
    .option("header", "true") \
    .option("inferSchema", "true") \
    .option("dataAddress", "'Sheet1'!A1") \
    .load("/mnt/datalake/raw/report.xlsx")
```

### From a Database (JDBC)

```python
df = spark.read.format("jdbc") \
    .option("url", "jdbc:postgresql://host:5432/mydb") \
    .option("dbtable", "public.employees") \
    .option("user", dbutils.secrets.get("scope", "db_user")) \
    .option("password", dbutils.secrets.get("scope", "db_pass")) \
    .option("driver", "org.postgresql.Driver") \
    .load()

# with query instead of full table
df = spark.read.format("jdbc") \
    .option("url", jdbc_url) \
    .option("query", "SELECT * FROM orders WHERE order_date > '2024-01-01'") \
    .option("user", user) \
    .option("password", password) \
    .load()

# parallel read (partitioned fetch)
df = spark.read.format("jdbc") \
    .option("url", jdbc_url) \
    .option("dbtable", "orders") \
    .option("partitionColumn", "order_id") \
    .option("lowerBound", "1") \
    .option("upperBound", "1000000") \
    .option("numPartitions", "10") \
    .option("user", user) \
    .option("password", password) \
    .load()
```

### From Cloud Storage Directly

```python
# Azure Blob / ADLS Gen2
df = spark.read.csv("abfss://container@storageaccount.dfs.core.windows.net/path/file.csv")

# AWS S3
df = spark.read.parquet("s3://bucket-name/path/data.parquet")

# Google Cloud Storage
df = spark.read.parquet("gs://bucket-name/path/data.parquet")
```

### Create DataFrame Manually

```python
from pyspark.sql import Row
from pyspark.sql.types import *

# from list of tuples
data = [("Alice", "Engineering", 95000), ("Bob", "Sales", 65000)]
df = spark.createDataFrame(data, ["name", "department", "salary"])

# from list of Rows
data = [Row(name="Alice", salary=95000), Row(name="Bob", salary=65000)]
df = spark.createDataFrame(data)

# from Pandas DataFrame
import pandas as pd
pdf = pd.DataFrame({"name": ["Alice", "Bob"], "salary": [95000, 65000]})
df = spark.createDataFrame(pdf)

# empty DataFrame with schema
schema = StructType([
    StructField("id", IntegerType()),
    StructField("name", StringType()),
])
df = spark.createDataFrame([], schema)
```

---

## 6. Viewing and Inspecting Data

```python
from pyspark.sql import functions as F

# --- preview data ---
df.show()                  # first 20 rows, truncated at 20 chars
df.show(50)                # first 50 rows
df.show(20, truncate=False)  # no column truncation
df.show(20, truncate=50)     # truncate at 50 chars
df.show(vertical=True)       # one column per line (wide tables)

# Databricks display (rich rendering with charts)
display(df)
display(df.limit(100))

# --- schema and types ---
df.printSchema()
# root
#  |-- name: string (nullable = true)
#  |-- salary: integer (nullable = true)
#  |-- hire_date: date (nullable = true)

df.dtypes                    # [("name", "string"), ("salary", "int"), ...]
df.columns                   # ["name", "salary", "hire_date"]
df.schema                    # StructType object

# --- shape and counts ---
df.count()                   # total row count (triggers full scan)
len(df.columns)              # number of columns

# --- statistics ---
df.describe().show()         # count, mean, stddev, min, max for numeric columns
df.summary().show()          # also includes 25%, 50%, 75%

# describe specific columns
df.describe("salary", "age").show()

# --- sample ---
df.limit(10)                         # first 10 rows (not random)
df.sample(fraction=0.1)             # 10% random sample
df.sample(fraction=0.01, seed=42)   # reproducible sample
df.head(5)                           # returns list of Row objects
df.first()                           # single Row
df.take(5)                           # list of 5 Rows
df.tail(5)                           # last 5 rows (requires full scan)
df.collect()                         # ALL rows to driver — use only on small data

# --- distinct and unique ---
df.distinct().count()                            # distinct rows
df.select("department").distinct().show()         # unique departments
df.select("department").distinct().count()        # count unique

# --- value counts ---
df.groupBy("department").count().orderBy(F.desc("count")).show()
df.groupBy("status").count().show()

# --- null counts ---
df.select([F.count(F.when(F.col(c).isNull(), c)).alias(c) for c in df.columns]).show()

# null percentage per column
total = df.count()
null_counts = df.select([
    (F.count(F.when(F.col(c).isNull(), 1)) / total * 100).alias(c)
    for c in df.columns
])
null_counts.show()

# --- column-level stats ---
df.select(
    F.min("salary").alias("min"),
    F.max("salary").alias("max"),
    F.mean("salary").alias("avg"),
    F.stddev("salary").alias("stddev"),
    F.count("salary").alias("non_null_count"),
    F.countDistinct("salary").alias("unique_count"),
).show()

# --- frequency distribution ---
df.groupBy("salary") \
    .count() \
    .orderBy(F.desc("count")) \
    .show(20)

# --- approximate distinct count (fast on large data) ---
df.select(F.approx_count_distinct("customer_id")).show()

# --- to Pandas (small datasets) ---
pdf = df.toPandas()
pdf = df.limit(10000).toPandas()
```

---

## 7. Writing Data — All Destinations

### Write Modes

| Mode | Behavior |
|------|----------|
| `overwrite` | Drop existing data and replace |
| `append` | Add rows to existing data |
| `ignore` | Do nothing if data already exists |
| `errorifexists` | Throw error if data exists (default) |

### Write to Delta Lake (Preferred)

```python
# write to a path
df.write.format("delta").mode("overwrite").save("/mnt/datalake/delta/sales")

# write as a managed table in the catalog
df.write.format("delta").mode("overwrite").saveAsTable("catalog.schema.sales")

# write with partitioning
df.write.format("delta") \
    .mode("overwrite") \
    .partitionBy("year", "month") \
    .save("/mnt/datalake/delta/sales")

# overwrite specific partitions only
df.write.format("delta") \
    .mode("overwrite") \
    .option("replaceWhere", "year = 2024 AND month = 3") \
    .save("/mnt/datalake/delta/sales")

# with options
df.write.format("delta") \
    .mode("overwrite") \
    .option("overwriteSchema", "true") \
    .option("mergeSchema", "true") \
    .saveAsTable("catalog.schema.sales")
```

### Write to Parquet

```python
df.write.parquet("/mnt/datalake/output/sales.parquet", mode="overwrite")
df.write.parquet("/mnt/output/", mode="overwrite", partitionBy=["year", "month"])
```

### Write to CSV

```python
df.write.csv("/mnt/datalake/output/sales.csv", header=True, mode="overwrite")

# single file output
df.coalesce(1).write.csv("/mnt/output/", header=True, mode="overwrite")
# note: creates a directory with one part file — rename manually if needed
```

### Write to JSON

```python
df.write.json("/mnt/output/events.json", mode="overwrite")
df.write.json("/mnt/output/events/", mode="overwrite", lineSep="\n")
```

### Write to a Database (JDBC)

```python
df.write.format("jdbc") \
    .option("url", "jdbc:postgresql://host:5432/mydb") \
    .option("dbtable", "public.sales_summary") \
    .option("user", user) \
    .option("password", password) \
    .option("driver", "org.postgresql.Driver") \
    .mode("overwrite") \
    .save()

# append mode for incremental loads
df.write.format("jdbc") \
    .option("url", jdbc_url) \
    .option("dbtable", "public.sales") \
    .mode("append") \
    .save()
```

### Write to Excel (via Pandas)

```python
pdf = df.toPandas()
pdf.to_excel("/dbfs/mnt/output/report.xlsx", index=False, sheet_name="Results")
```

---

## 8. Working with Unity Catalog and Schemas

Unity Catalog uses a three-level namespace: `catalog.schema.table`.

```sql
-- %sql
-- list catalogs
SHOW CATALOGS;

-- use a catalog
USE CATALOG my_catalog;

-- list schemas
SHOW SCHEMAS;
SHOW SCHEMAS IN my_catalog;

-- use a schema
USE SCHEMA my_schema;
USE my_catalog.my_schema;

-- list tables
SHOW TABLES;
SHOW TABLES IN my_catalog.my_schema;

-- describe table
DESCRIBE TABLE my_catalog.my_schema.sales;
DESCRIBE TABLE EXTENDED my_catalog.my_schema.sales;
DESCRIBE DETAIL my_catalog.my_schema.sales;
DESCRIBE HISTORY my_catalog.my_schema.sales;

-- table metadata
SHOW CREATE TABLE my_catalog.my_schema.sales;
SHOW COLUMNS IN my_catalog.my_schema.sales;
SHOW TBLPROPERTIES my_catalog.my_schema.sales;
```

```python
# PySpark equivalents
spark.catalog.listCatalogs()
spark.catalog.listDatabases()       # schemas
spark.catalog.listTables("my_schema")
spark.catalog.tableExists("catalog.schema.sales")

# create schema
spark.sql("CREATE SCHEMA IF NOT EXISTS my_catalog.new_schema")

# create table
spark.sql("""
    CREATE TABLE IF NOT EXISTS my_catalog.my_schema.employees (
        id INT,
        name STRING,
        department STRING,
        salary DOUBLE,
        hire_date DATE
    )
    USING DELTA
    COMMENT 'Employee records'
""")
```

---

# Part 3: DataFrame Operations

---

## 9. Selecting Columns

```python
from pyspark.sql import functions as F

# select by name
df.select("name", "salary")
df.select(F.col("name"), F.col("salary"))

# select with alias
df.select(
    F.col("name").alias("employee_name"),
    (F.col("salary") * 12).alias("annual_salary"),
)

# select by list
cols = ["name", "department", "salary"]
df.select(cols)
df.select(*cols)

# select all columns
df.select("*")

# select with expression
df.selectExpr("name", "salary * 12 AS annual_salary", "UPPER(department) AS dept")

# select columns by pattern
import fnmatch
date_cols = [c for c in df.columns if fnmatch.fnmatch(c, "*date*")]
df.select(date_cols)

# select columns by type
string_cols = [f.name for f in df.schema.fields if isinstance(f.dataType, StringType)]
df.select(string_cols)

# select all except some
exclude = {"temp_col", "debug_col"}
df.select([c for c in df.columns if c not in exclude])
```

---

## 10. Filtering Rows

```python
# single condition
df.filter(F.col("salary") > 80000)
df.where(F.col("salary") > 80000)     # same as filter

# equals
df.filter(F.col("department") == "Engineering")

# not equals
df.filter(F.col("department") != "Sales")

# AND (use &)
df.filter((F.col("salary") > 80000) & (F.col("department") == "Engineering"))

# OR (use |)
df.filter((F.col("department") == "Sales") | (F.col("department") == "HR"))

# NOT (use ~)
df.filter(~(F.col("department") == "HR"))

# IN list
df.filter(F.col("department").isin("Engineering", "Sales", "Marketing"))
df.filter(F.col("department").isin(["Engineering", "Sales"]))

# NOT IN
df.filter(~F.col("department").isin("HR", "Finance"))

# BETWEEN
df.filter(F.col("salary").between(50000, 100000))

# NULL checks
df.filter(F.col("manager_id").isNull())
df.filter(F.col("manager_id").isNotNull())

# LIKE (string pattern)
df.filter(F.col("name").like("A%"))          # starts with A
df.filter(F.col("name").like("%smith%"))     # contains smith
df.filter(F.col("email").like("%@gmail.com"))

# RLIKE (regex)
df.filter(F.col("email").rlike(r"^[\w.]+@gmail\.com$"))

# contains
df.filter(F.col("name").contains("Smith"))

# startswith / endswith
df.filter(F.col("name").startswith("A"))
df.filter(F.col("email").endswith("@company.com"))

# date filters
df.filter(F.col("hire_date") > "2023-01-01")
df.filter(F.col("hire_date").between("2023-01-01", "2023-12-31"))
df.filter(F.year("hire_date") == 2024)

# column-to-column comparison
df.filter(F.col("salary") > F.col("target_salary"))

# chain multiple filters
result = (
    df
    .filter(F.col("status") == "active")
    .filter(F.col("salary") > 50000)
    .filter(F.col("hire_date") >= "2020-01-01")
)

# SQL expression filter
df.filter("salary > 80000 AND department = 'Engineering'")
```

---

## 11. Adding and Computing New Columns

```python
# add a constant column
df = df.withColumn("country", F.lit("USA"))
df = df.withColumn("load_timestamp", F.current_timestamp())
df = df.withColumn("source_file", F.input_file_name())

# computed column
df = df.withColumn("annual_salary", F.col("salary") * 12)
df = df.withColumn("full_name", F.concat(F.col("first_name"), F.lit(" "), F.col("last_name")))

# conditional column (CASE WHEN equivalent)
df = df.withColumn("tier",
    F.when(F.col("salary") >= 100000, "Senior")
     .when(F.col("salary") >= 70000, "Mid")
     .when(F.col("salary") >= 40000, "Junior")
     .otherwise("Entry")
)

# boolean column
df = df.withColumn("is_senior", F.col("salary") >= 100000)
df = df.withColumn("is_weekend", F.dayofweek("order_date").isin(1, 7))

# from existing column transformation
df = df.withColumn("email_domain", F.split(F.col("email"), "@")[1])
df = df.withColumn("name_upper", F.upper(F.col("name")))
df = df.withColumn("salary_rounded", F.round(F.col("salary"), -3))

# multiple columns at once (Spark 3.3+)
df = df.withColumns({
    "annual_salary": F.col("salary") * 12,
    "tax": F.col("salary") * 0.3,
    "net_salary": F.col("salary") * 0.7,
    "load_date": F.current_date(),
})

# row number / unique ID
from pyspark.sql.window import Window
df = df.withColumn("row_id", F.monotonically_increasing_id())

# hash column (for surrogate keys)
df = df.withColumn("hash_key", F.sha2(F.concat_ws("||", "id", "name", "date"), 256))
df = df.withColumn("hash_key", F.md5(F.concat_ws("||", "col1", "col2")))
```

---

## 12. Renaming Columns

```python
# rename one column
df = df.withColumnRenamed("old_name", "new_name")

# rename multiple columns
df = df.withColumnRenamed("emp_nm", "employee_name") \
       .withColumnRenamed("dept", "department") \
       .withColumnRenamed("sal", "salary")

# rename all columns at once
new_names = ["id", "name", "department", "salary"]
df = df.toDF(*new_names)

# clean column names (lowercase, replace spaces)
for col_name in df.columns:
    clean = col_name.lower().replace(" ", "_").replace("(", "").replace(")", "")
    df = df.withColumnRenamed(col_name, clean)

# using alias in select
df = df.select(
    F.col("emp_nm").alias("employee_name"),
    F.col("dept").alias("department"),
    F.col("sal").alias("salary"),
)

# add prefix/suffix to all columns
df = df.select([F.col(c).alias(f"src_{c}") for c in df.columns])
df = df.select([F.col(c).alias(f"{c}_v2") for c in df.columns])
```

---

## 13. Dropping Columns and Rows

### Dropping Columns

```python
# drop single column
df = df.drop("temp_col")

# drop multiple columns
df = df.drop("temp1", "temp2", "debug_col")

# drop by list
cols_to_drop = ["col1", "col2", "col3"]
df = df.drop(*cols_to_drop)

# drop columns by pattern
drop_cols = [c for c in df.columns if c.startswith("_") or c.startswith("tmp_")]
df = df.drop(*drop_cols)

# keep only certain columns (effectively dropping the rest)
df = df.select("id", "name", "salary")
```

### Dropping Rows

```python
# drop rows with any null
df = df.dropna()
df = df.na.drop()

# drop rows where specific columns are null
df = df.dropna(subset=["email", "phone"])

# drop rows where ALL columns are null
df = df.dropna(how="all")

# keep rows with at least N non-null values
df = df.dropna(thresh=5)

# drop rows by condition (filter = keep, so negate to drop)
df = df.filter(F.col("salary") > 0)     # drops rows with salary <= 0
df = df.filter(F.col("status") != "deleted")
```

---

## 14. Data Types and Casting

### PySpark Data Types

| PySpark Type | SQL Equivalent | Python Equivalent |
|-------------|----------------|-------------------|
| `StringType()` | STRING | str |
| `IntegerType()` | INT | int |
| `LongType()` | BIGINT | int |
| `FloatType()` | FLOAT | float |
| `DoubleType()` | DOUBLE | float |
| `DecimalType(p, s)` | DECIMAL(p, s) | Decimal |
| `BooleanType()` | BOOLEAN | bool |
| `DateType()` | DATE | datetime.date |
| `TimestampType()` | TIMESTAMP | datetime.datetime |
| `ArrayType(elem)` | ARRAY<elem> | list |
| `MapType(k, v)` | MAP<k, v> | dict |
| `StructType(fields)` | STRUCT | Row |
| `BinaryType()` | BINARY | bytes |

### Casting

```python
# cast with .cast()
df = df.withColumn("salary", F.col("salary").cast("double"))
df = df.withColumn("id", F.col("id").cast(IntegerType()))
df = df.withColumn("date", F.col("date_str").cast("date"))
df = df.withColumn("amount", F.col("amount_str").cast(DecimalType(10, 2)))

# cast multiple columns
for col_name in ["price", "tax", "total"]:
    df = df.withColumn(col_name, F.col(col_name).cast("double"))

# string to date
df = df.withColumn("date", F.to_date("date_str", "yyyy-MM-dd"))
df = df.withColumn("date", F.to_date("date_str", "dd/MM/yyyy"))
df = df.withColumn("date", F.to_date("date_str", "MM-dd-yyyy"))

# string to timestamp
df = df.withColumn("ts", F.to_timestamp("ts_str", "yyyy-MM-dd HH:mm:ss"))

# date to string
df = df.withColumn("date_str", F.date_format("date", "yyyy-MM-dd"))
df = df.withColumn("month_str", F.date_format("date", "MMMM yyyy"))

# check types
df.printSchema()
df.dtypes
df.schema["salary"].dataType   # DoubleType()
```

---

## 15. Handling Null and Missing Values

```python
# --- detect nulls ---
df.filter(F.col("email").isNull())
df.filter(F.col("email").isNotNull())

# count nulls per column
df.select([F.count(F.when(F.col(c).isNull(), 1)).alias(c) for c in df.columns]).show()

# null percentage
total = df.count()
df.select([
    F.round(F.count(F.when(F.col(c).isNull(), 1)) / total * 100, 2).alias(c)
    for c in df.columns
]).show()

# --- fill nulls ---
# fill all nulls in all columns with same type
df = df.fillna(0)                  # numeric columns
df = df.fillna("Unknown")         # string columns
df = df.fillna(False)             # boolean columns

# fill specific columns
df = df.fillna({"salary": 0, "department": "Unknown", "is_active": True})

# fill with column-level logic
df = df.withColumn("salary", F.coalesce(F.col("salary"), F.lit(0)))
df = df.withColumn("name", F.coalesce(F.col("name"), F.lit("N/A")))

# fill with another column
df = df.withColumn("phone", F.coalesce(F.col("phone"), F.col("mobile"), F.lit("No Contact")))

# --- replace values ---
df = df.replace("N/A", None)                        # replace string with null
df = df.replace(["NA", "N/A", "", "-"], None)       # replace multiple
df = df.replace({"M": "Male", "F": "Female"}, subset=["gender"])

# --- nanvl (handle NaN for floats) ---
df = df.withColumn("value", F.nanvl(F.col("value"), F.lit(0.0)))

# --- drop rows with nulls ---
df = df.dropna()                          # any null
df = df.dropna(subset=["id", "name"])     # nulls in specific columns
df = df.dropna(how="all")                 # all columns null
df = df.dropna(thresh=3)                  # at least 3 non-null values
```

---

## 16. String Functions

```python
# --- case ---
df = df.withColumn("name_upper", F.upper("name"))
df = df.withColumn("name_lower", F.lower("name"))
df = df.withColumn("name_title", F.initcap("name"))

# --- trim ---
df = df.withColumn("name", F.trim("name"))
df = df.withColumn("name", F.ltrim("name"))
df = df.withColumn("name", F.rtrim("name"))

# --- length ---
df = df.withColumn("name_len", F.length("name"))

# --- substring ---
df = df.withColumn("first_3", F.substring("name", 1, 3))   # 1-based index
df = df.withColumn("area_code", F.substring("phone", 1, 3))

# --- concat ---
df = df.withColumn("full_name", F.concat("first_name", F.lit(" "), "last_name"))
df = df.withColumn("address", F.concat_ws(", ", "city", "state", "zip"))

# --- split ---
df = df.withColumn("domain", F.split("email", "@")[1])
df = df.withColumn("parts", F.split("full_name", " "))     # returns array

# --- replace ---
df = df.withColumn("phone_clean", F.regexp_replace("phone", r"[^0-9]", ""))
df = df.withColumn("text", F.regexp_replace("text", r"\s+", " "))
df = df.withColumn("name", F.translate("name", "áéíóú", "aeiou"))

# --- extract with regex ---
df = df.withColumn("year", F.regexp_extract("date_str", r"(\d{4})", 1))
df = df.withColumn("email_user", F.regexp_extract("email", r"^([\w.]+)@", 1))

# --- pattern matching ---
df.filter(F.col("name").like("A%"))          # SQL LIKE
df.filter(F.col("name").rlike(r"^[A-Z]"))    # regex

# --- padding ---
df = df.withColumn("id_padded", F.lpad("id", 8, "0"))      # "42" → "00000042"
df = df.withColumn("code", F.rpad("code", 10, " "))

# --- position ---
df = df.withColumn("at_pos", F.instr("email", "@"))        # 1-based index, 0 if not found

# --- repeat ---
df = df.withColumn("dashes", F.repeat(F.lit("-"), 20))

# --- reverse ---
df = df.withColumn("name_rev", F.reverse("name"))

# --- decode / encode ---
df = df.withColumn("decoded", F.decode("binary_col", "UTF-8"))
df = df.withColumn("encoded", F.encode("text_col", "UTF-8"))
```

---

## 17. Date and Timestamp Functions

```python
# --- current date/time ---
df = df.withColumn("today", F.current_date())
df = df.withColumn("now", F.current_timestamp())

# --- extracting parts ---
df = df.withColumn("year", F.year("order_date"))
df = df.withColumn("month", F.month("order_date"))
df = df.withColumn("day", F.dayofmonth("order_date"))
df = df.withColumn("hour", F.hour("timestamp_col"))
df = df.withColumn("minute", F.minute("timestamp_col"))
df = df.withColumn("weekday", F.dayofweek("order_date"))          # 1=Sun, 7=Sat
df = df.withColumn("day_of_year", F.dayofyear("order_date"))
df = df.withColumn("week_of_year", F.weekofyear("order_date"))
df = df.withColumn("quarter", F.quarter("order_date"))
df = df.withColumn("last_day", F.last_day("order_date"))          # last day of month

# --- formatting ---
df = df.withColumn("date_str", F.date_format("order_date", "yyyy-MM-dd"))
df = df.withColumn("month_name", F.date_format("order_date", "MMMM"))
df = df.withColumn("day_name", F.date_format("order_date", "EEEE"))
df = df.withColumn("yyyymm", F.date_format("order_date", "yyyyMM"))

# --- arithmetic ---
df = df.withColumn("next_week", F.date_add("order_date", 7))
df = df.withColumn("last_month", F.add_months("order_date", -1))
df = df.withColumn("days_diff", F.datediff("end_date", "start_date"))
df = df.withColumn("months_diff", F.months_between("end_date", "start_date"))

# --- truncation ---
df = df.withColumn("month_start", F.trunc("order_date", "MM"))       # to month
df = df.withColumn("year_start", F.trunc("order_date", "yyyy"))      # to year
df = df.withColumn("week_start", F.trunc("order_date", "week"))      # to Monday
df = df.withColumn("ts_hour", F.date_trunc("hour", "timestamp_col")) # to hour

# --- next/previous day of week ---
df = df.withColumn("next_monday", F.next_day("order_date", "Mon"))

# --- parsing strings to dates ---
df = df.withColumn("date", F.to_date("date_str", "yyyy-MM-dd"))
df = df.withColumn("date", F.to_date("date_str", "dd/MM/yyyy"))
df = df.withColumn("date", F.to_date("date_str", "MMM dd, yyyy"))

# --- Unix timestamps ---
df = df.withColumn("epoch", F.unix_timestamp("timestamp_col"))
df = df.withColumn("ts", F.from_unixtime("epoch_col", "yyyy-MM-dd HH:mm:ss"))

# --- generate date range ---
from pyspark.sql.functions import sequence, explode
date_range = spark.sql("""
    SELECT explode(sequence(
        to_date('2024-01-01'),
        to_date('2024-12-31'),
        interval 1 day
    )) AS date
""")
```

---

## 18. Sorting and Ordering

```python
# ascending (default)
df = df.orderBy("salary")
df = df.orderBy(F.col("salary").asc())
df = df.sort("salary")

# descending
df = df.orderBy(F.col("salary").desc())
df = df.orderBy(F.desc("salary"))

# multiple columns
df = df.orderBy(F.col("department").asc(), F.col("salary").desc())

# nulls first / last
df = df.orderBy(F.col("salary").asc_nulls_first())
df = df.orderBy(F.col("salary").desc_nulls_last())

# sort within partitions (for optimized writes)
df = df.sortWithinPartitions("department", "salary")
```

---

## 19. Grouping and Aggregation

```python
# basic aggregation
df.groupBy("department").count().show()
df.groupBy("department").sum("salary").show()
df.groupBy("department").avg("salary").show()

# multiple aggregations with agg()
from pyspark.sql import functions as F

result = df.groupBy("department").agg(
    F.count("*").alias("headcount"),
    F.avg("salary").alias("avg_salary"),
    F.sum("salary").alias("total_salary"),
    F.min("salary").alias("min_salary"),
    F.max("salary").alias("max_salary"),
    F.stddev("salary").alias("stddev_salary"),
    F.countDistinct("level").alias("unique_levels"),
    F.first("manager").alias("first_manager"),
    F.collect_list("name").alias("employee_names"),
    F.collect_set("level").alias("unique_levels_list"),
)

# group by multiple columns
df.groupBy("department", "level").agg(
    F.count("*").alias("count"),
    F.avg("salary").alias("avg_salary"),
)

# group by expression
df.groupBy(F.year("hire_date").alias("hire_year")).agg(
    F.count("*").alias("hires"),
)

# --- percentile (approximate) ---
df.groupBy("department").agg(
    F.percentile_approx("salary", 0.5).alias("median_salary"),
    F.percentile_approx("salary", [0.25, 0.5, 0.75]).alias("quartiles"),
)

# --- rollup (subtotals) ---
df.rollup("department", "level").agg(
    F.count("*").alias("count"),
    F.avg("salary").alias("avg_salary"),
).orderBy("department", "level").show()

# --- cube (all combinations) ---
df.cube("department", "level").agg(
    F.count("*").alias("count"),
).show()

# --- aggregate entire DataFrame (no groups) ---
df.agg(
    F.count("*").alias("total"),
    F.avg("salary").alias("avg_salary"),
).show()
```

---

## 20. Joins

```python
# inner join (default)
result = df1.join(df2, on="customer_id", how="inner")

# left join
result = df1.join(df2, on="customer_id", how="left")

# right join
result = df1.join(df2, on="customer_id", how="right")

# full outer join
result = df1.join(df2, on="customer_id", how="outer")

# cross join
result = df1.crossJoin(df2)

# left anti join (rows in df1 NOT in df2)
result = df1.join(df2, on="customer_id", how="left_anti")

# left semi join (rows in df1 that HAVE a match in df2, no df2 columns)
result = df1.join(df2, on="customer_id", how="left_semi")

# join on multiple columns
result = df1.join(df2, on=["year", "month", "product"], how="inner")

# join on different column names
result = df1.join(df2, df1["cust_id"] == df2["customer_id"], how="left")
# drop the duplicate column after
result = result.drop(df2["customer_id"])

# join with complex condition
result = df1.join(
    df2,
    (df1["id"] == df2["id"]) & (df1["date"] >= df2["start_date"]) & (df1["date"] <= df2["end_date"]),
    how="inner"
)

# self join
managers = df.alias("e").join(
    df.alias("m"),
    F.col("e.manager_id") == F.col("m.id"),
    how="left"
).select(
    F.col("e.name").alias("employee"),
    F.col("m.name").alias("manager"),
)

# broadcast join (force small table to broadcast)
from pyspark.sql.functions import broadcast
result = df_large.join(broadcast(df_small), on="key", how="inner")
```

### When to Use Each Join

| Join Type | Use Case |
|-----------|----------|
| `inner` | Only matching rows from both sides |
| `left` | All rows from left, matching from right (NULLs for no match) |
| `left_anti` | Rows in left with NO match in right (anti-join / NOT EXISTS) |
| `left_semi` | Rows in left WITH a match in right (EXISTS, no right columns) |
| `outer` | All rows from both sides |
| `cross` | Every combination (Cartesian product) |

---

## 21. Unions and Set Operations

```python
# union (keeps duplicates, must have same schema)
combined = df1.union(df2)
combined = df1.unionAll(df2)    # same as union

# union by column name (handles different column order)
combined = df1.unionByName(df2)
combined = df1.unionByName(df2, allowMissingColumns=True)  # fills missing with null

# union multiple DataFrames
from functools import reduce
dfs = [df1, df2, df3, df4]
combined = reduce(lambda a, b: a.unionByName(b, allowMissingColumns=True), dfs)

# distinct after union (like SQL UNION without ALL)
combined = df1.union(df2).distinct()

# intersect (rows in both)
common = df1.intersect(df2)
common = df1.intersectAll(df2)    # preserves duplicates

# except / subtract (rows in df1 not in df2)
diff = df1.subtract(df2)
diff = df1.exceptAll(df2)         # preserves duplicates
```

---

## 22. Window Functions

```python
from pyspark.sql.window import Window
from pyspark.sql import functions as F

# --- define window ---
w = Window.partitionBy("department").orderBy(F.desc("salary"))

# --- ranking ---
df = df.withColumn("rank", F.rank().over(w))
df = df.withColumn("dense_rank", F.dense_rank().over(w))
df = df.withColumn("row_number", F.row_number().over(w))
df = df.withColumn("ntile", F.ntile(4).over(w))         # quartiles
df = df.withColumn("percent_rank", F.percent_rank().over(w))
df = df.withColumn("cume_dist", F.cume_dist().over(w))

# top N per group
w = Window.partitionBy("department").orderBy(F.desc("salary"))
df_top3 = df.withColumn("rn", F.row_number().over(w)).filter(F.col("rn") <= 3).drop("rn")

# --- aggregate windows (no ORDER BY needed) ---
w_dept = Window.partitionBy("department")
df = df.withColumn("dept_avg", F.avg("salary").over(w_dept))
df = df.withColumn("dept_total", F.sum("salary").over(w_dept))
df = df.withColumn("dept_count", F.count("*").over(w_dept))
df = df.withColumn("dept_max", F.max("salary").over(w_dept))
df = df.withColumn("dept_min", F.min("salary").over(w_dept))

# percent of group total
df = df.withColumn("pct_of_dept", F.col("salary") / F.sum("salary").over(w_dept))

# difference from group mean
df = df.withColumn("diff_from_avg", F.col("salary") - F.avg("salary").over(w_dept))

# --- lag and lead ---
w_ordered = Window.partitionBy("customer_id").orderBy("order_date")
df = df.withColumn("prev_order_amount", F.lag("amount", 1).over(w_ordered))
df = df.withColumn("next_order_amount", F.lead("amount", 1).over(w_ordered))
df = df.withColumn("order_change", F.col("amount") - F.lag("amount", 1).over(w_ordered))

# lag with default value
df = df.withColumn("prev_amount", F.lag("amount", 1, 0).over(w_ordered))

# --- running totals ---
w_running = Window.partitionBy("customer_id").orderBy("order_date") \
    .rowsBetween(Window.unboundedPreceding, Window.currentRow)
df = df.withColumn("running_total", F.sum("amount").over(w_running))
df = df.withColumn("cumulative_count", F.count("*").over(w_running))

# --- moving average ---
w_moving = Window.partitionBy("product").orderBy("date") \
    .rowsBetween(-6, Window.currentRow)    # 7-day window
df = df.withColumn("ma_7", F.avg("revenue").over(w_moving))

# --- first / last value ---
df = df.withColumn("first_order", F.first("order_date").over(w_dept))
df = df.withColumn("last_order", F.last("order_date").over(w_dept))

# --- window frame specifications ---
# ROWS BETWEEN:
Window.rowsBetween(Window.unboundedPreceding, Window.currentRow)    # all rows up to current
Window.rowsBetween(-3, 0)                                            # current + 3 preceding
Window.rowsBetween(-3, 3)                                            # 3 before + 3 after
Window.rowsBetween(Window.unboundedPreceding, Window.unboundedFollowing)  # entire partition

# RANGE BETWEEN (value-based):
Window.rangeBetween(Window.unboundedPreceding, Window.currentRow)
```

---

## 23. Pivot and Unpivot

### Pivot (Rows to Columns)

```python
# pivot: aggregate revenue per product per quarter
pivoted = df.groupBy("product").pivot("quarter", ["Q1", "Q2", "Q3", "Q4"]).sum("revenue")

# pivot without specifying values (auto-discovers, slower)
pivoted = df.groupBy("product").pivot("quarter").sum("revenue")

# pivot with multiple aggregations
pivoted = df.groupBy("product").pivot("quarter").agg(
    F.sum("revenue").alias("revenue"),
    F.count("*").alias("orders"),
)
```

### Unpivot (Columns to Rows)

```python
# Spark 3.4+ : unpivot
unpivoted = pivoted.unpivot(
    ids=["product"],
    values=["Q1", "Q2", "Q3", "Q4"],
    variableColumnName="quarter",
    valueColumnName="revenue",
)

# manual unpivot with stack (works in all Spark versions)
unpivoted = pivoted.selectExpr(
    "product",
    "stack(4, 'Q1', Q1, 'Q2', Q2, 'Q3', Q3, 'Q4', Q4) AS (quarter, revenue)"
).filter(F.col("revenue").isNotNull())

# manual with union
unpivoted = (
    pivoted.select("product", F.lit("Q1").alias("quarter"), F.col("Q1").alias("revenue"))
    .union(pivoted.select("product", F.lit("Q2"), F.col("Q2")))
    .union(pivoted.select("product", F.lit("Q3"), F.col("Q3")))
    .union(pivoted.select("product", F.lit("Q4"), F.col("Q4")))
)
```

---

## 24. Deduplication

```python
# drop exact duplicate rows
df = df.dropDuplicates()
df = df.distinct()

# drop duplicates on specific columns (keeps first occurrence)
df = df.dropDuplicates(["email"])
df = df.dropDuplicates(["customer_id", "order_date"])

# keep the most recent record per customer
w = Window.partitionBy("customer_id").orderBy(F.desc("updated_at"))
df = df.withColumn("rn", F.row_number().over(w)).filter(F.col("rn") == 1).drop("rn")

# keep the row with highest salary per department
w = Window.partitionBy("department").orderBy(F.desc("salary"))
df_top = df.withColumn("rn", F.row_number().over(w)).filter(F.col("rn") == 1).drop("rn")

# count duplicates
df.groupBy("email").count().filter(F.col("count") > 1).show()

# view duplicate rows
df.groupBy(df.columns).count().filter(F.col("count") > 1).show()
```

---

## 25. Conditional Logic — WHEN, OTHERWISE, COALESCE

```python
# --- WHEN / OTHERWISE (CASE WHEN equivalent) ---
df = df.withColumn("tier",
    F.when(F.col("salary") >= 100000, "Senior")
     .when(F.col("salary") >= 70000, "Mid")
     .when(F.col("salary") >= 40000, "Junior")
     .otherwise("Entry")
)

# nested conditions
df = df.withColumn("status",
    F.when((F.col("active") == True) & (F.col("verified") == True), "Active Verified")
     .when(F.col("active") == True, "Active Unverified")
     .otherwise("Inactive")
)

# conditional aggregation
df.groupBy("department").agg(
    F.count(F.when(F.col("salary") >= 100000, 1)).alias("senior_count"),
    F.count(F.when(F.col("salary") < 50000, 1)).alias("junior_count"),
    F.sum(F.when(F.col("status") == "active", F.col("salary")).otherwise(0)).alias("active_payroll"),
)

# --- COALESCE (first non-null value) ---
df = df.withColumn("contact", F.coalesce("phone", "mobile", "email", F.lit("No Contact")))

# --- NULLIF equivalent ---
df = df.withColumn("value", F.when(F.col("value") == 0, None).otherwise(F.col("value")))

# --- GREATEST / LEAST ---
df = df.withColumn("max_score", F.greatest("score1", "score2", "score3"))
df = df.withColumn("min_score", F.least("score1", "score2", "score3"))

# --- IF equivalent ---
df = df.withColumn("label", F.expr("IF(salary > 100000, 'High', 'Standard')"))
```

---

## 26. Working with Arrays and Structs

```python
# --- arrays ---
# create array column
df = df.withColumn("scores", F.array("score1", "score2", "score3"))

# access array elements
df = df.withColumn("first_score", F.col("scores")[0])
df = df.withColumn("last_score", F.element_at("scores", -1))

# array functions
df = df.withColumn("num_scores", F.size("scores"))
df = df.withColumn("has_100", F.array_contains("scores", 100))
df = df.withColumn("sorted", F.sort_array("scores"))
df = df.withColumn("reversed", F.reverse("scores"))
df = df.withColumn("unique", F.array_distinct("scores"))
df = df.withColumn("flat", F.flatten("nested_array"))

# explode (one row per element)
df_exploded = df.select("name", F.explode("scores").alias("score"))

# posexplode (with index)
df_exploded = df.select("name", F.posexplode("scores").alias("index", "score"))

# explode_outer (keeps rows with null/empty arrays)
df_exploded = df.select("name", F.explode_outer("scores").alias("score"))

# collect back into array
df_grouped = df.groupBy("department").agg(F.collect_list("name").alias("employees"))
df_grouped = df.groupBy("department").agg(F.collect_set("level").alias("unique_levels"))

# array set operations
df = df.withColumn("common", F.array_intersect("skills", "required_skills"))
df = df.withColumn("missing", F.array_except("required_skills", "skills"))
df = df.withColumn("all_skills", F.array_union("skills", "bonus_skills"))

# --- structs ---
df = df.withColumn("address", F.struct("city", "state", "zip"))
df = df.withColumn("city", F.col("address.city"))
df = df.withColumn("state", F.col("address")["state"])
```

---

## 27. Working with JSON Columns

```python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

# --- parse JSON string column ---
json_schema = StructType([
    StructField("user", StringType()),
    StructField("action", StringType()),
    StructField("metadata", StructType([
        StructField("ip", StringType()),
        StructField("browser", StringType()),
    ]))
])
df = df.withColumn("parsed", F.from_json("json_col", json_schema))
df = df.withColumn("user", F.col("parsed.user"))
df = df.withColumn("ip", F.col("parsed.metadata.ip"))

# auto-infer schema from sample
sample_json = df.select("json_col").head()[0]
inferred_schema = spark.read.json(spark.sparkContext.parallelize([sample_json])).schema
df = df.withColumn("parsed", F.from_json("json_col", inferred_schema))

# --- extract without full parsing ---
df = df.withColumn("user", F.get_json_object("json_col", "$.user"))
df = df.withColumn("ip", F.get_json_object("json_col", "$.metadata.ip"))

# json_tuple (extract multiple fields at once)
df = df.select("*", F.json_tuple("json_col", "user", "action").alias("user", "action"))

# --- convert struct to JSON string ---
df = df.withColumn("json_str", F.to_json(F.struct("name", "department", "salary")))

# --- schema_of_json (infer schema from example) ---
schema = F.schema_of_json('{"user": "alice", "action": "login"}')
df = df.withColumn("parsed", F.from_json("json_col", schema))
```

---

## 28. User Defined Functions (UDFs)

```python
from pyspark.sql.functions import udf, pandas_udf
from pyspark.sql.types import StringType, IntegerType, DoubleType

# --- standard UDF (Python, row-at-a-time, slow) ---
@udf(returnType=StringType())
def clean_phone(phone):
    if phone is None:
        return None
    digits = ''.join(c for c in phone if c.isdigit())
    return digits[-10:] if len(digits) >= 10 else None

df = df.withColumn("phone_clean", clean_phone(F.col("phone")))

# lambda UDF
email_domain = udf(lambda email: email.split("@")[1] if email and "@" in email else None, StringType())
df = df.withColumn("domain", email_domain(F.col("email")))

# --- Pandas UDF (vectorized, fast) ---
@pandas_udf(DoubleType())
def zscore(salary: pd.Series) -> pd.Series:
    return (salary - salary.mean()) / salary.std()

df = df.withColumn("salary_zscore", zscore(F.col("salary")))

# grouped Pandas UDF
from pyspark.sql.functions import pandas_udf, PandasUDFType

@pandas_udf("double", PandasUDFType.GROUPED_AGG)
def median_udf(values: pd.Series) -> float:
    return values.median()

df.groupBy("department").agg(median_udf("salary").alias("median_salary"))

# --- prefer built-in functions over UDFs ---
# UDFs serialize data to Python, which is 10-100x slower than native Spark functions.
# Always check if a built-in function exists before writing a UDF.
```

---

# Part 4: SQL in Databricks

---

## 29. Running SQL in Notebooks

```sql
-- use %sql magic command at the top of a cell
%sql

-- query a table
SELECT department, COUNT(*) AS headcount, AVG(salary) AS avg_salary
FROM catalog.schema.employees
GROUP BY department
ORDER BY avg_salary DESC;

-- create temp view from SQL
CREATE OR REPLACE TEMP VIEW high_earners AS
SELECT * FROM catalog.schema.employees WHERE salary > 100000;

-- DDL operations
CREATE TABLE IF NOT EXISTS catalog.schema.summary (
    department STRING,
    avg_salary DOUBLE,
    updated_at TIMESTAMP
) USING DELTA;

-- CTAS (Create Table As Select)
CREATE OR REPLACE TABLE catalog.schema.dept_summary AS
SELECT department, AVG(salary) AS avg_salary, COUNT(*) AS headcount
FROM catalog.schema.employees
GROUP BY department;

-- describe
DESCRIBE TABLE EXTENDED catalog.schema.employees;
DESCRIBE HISTORY catalog.schema.employees;

-- show table properties
SHOW TBLPROPERTIES catalog.schema.employees;
```

---

## 30. Mixing SQL and PySpark

```python
# PySpark → SQL: register DataFrame as temp view
df.createOrReplaceTempView("employees")
df.createOrReplaceGlobalTempView("employees_global")  # accessible across notebooks

# then query with SQL
result = spark.sql("""
    SELECT department, AVG(salary) AS avg_salary
    FROM employees
    WHERE hire_date >= '2020-01-01'
    GROUP BY department
    HAVING AVG(salary) > 70000
    ORDER BY avg_salary DESC
""")
result.show()

# SQL → PySpark: result is a DataFrame
df_result = spark.sql("SELECT * FROM employees WHERE salary > 100000")
df_result = df_result.withColumn("bonus", F.col("salary") * 0.1)

# use Python variables in SQL
min_salary = 80000
department = "Engineering"
result = spark.sql(f"""
    SELECT * FROM employees
    WHERE salary > {min_salary}
    AND department = '{department}'
""")

# parameterized SQL (safer)
result = spark.sql(
    "SELECT * FROM employees WHERE salary > :min_sal AND department = :dept",
    args={"min_sal": 80000, "dept": "Engineering"}
)

# execute SQL expression inline
df = df.withColumn("tier", F.expr("CASE WHEN salary > 100000 THEN 'Senior' ELSE 'Standard' END"))
df = df.selectExpr("*", "salary * 12 AS annual_salary")
```

---

# Part 5: Delta Lake

---

## 31. What is Delta Lake

Delta Lake is the default storage format in Databricks. It adds:

- **ACID transactions** — reads never see partial writes
- **Schema enforcement** — rejects writes that don't match the schema
- **Schema evolution** — safely add/modify columns
- **Time travel** — query historical versions
- **MERGE** — upserts, SCD, CDC
- **OPTIMIZE / VACUUM** — compaction and cleanup
- **Change Data Feed** — track row-level changes

All tables created in Databricks default to Delta format.

---

## 32. Creating and Managing Delta Tables

```sql
-- create managed table
CREATE TABLE IF NOT EXISTS catalog.schema.employees (
    id INT,
    name STRING,
    department STRING,
    salary DOUBLE,
    hire_date DATE
)
USING DELTA
COMMENT 'Employee records'
TBLPROPERTIES ('delta.autoOptimize.optimizeWrite' = 'true');

-- CTAS
CREATE OR REPLACE TABLE catalog.schema.active_employees AS
SELECT * FROM catalog.schema.employees WHERE status = 'active';

-- create from DataFrame
df.write.format("delta").mode("overwrite").saveAsTable("catalog.schema.employees")

-- partitioned table
CREATE TABLE catalog.schema.orders (
    order_id INT,
    customer_id INT,
    amount DOUBLE,
    order_date DATE
)
USING DELTA
PARTITIONED BY (order_date)
COMMENT 'Order transactions';
```

```python
# PySpark create
df.write.format("delta") \
    .mode("overwrite") \
    .option("overwriteSchema", "true") \
    .saveAsTable("catalog.schema.employees")

# insert/append
df_new.write.format("delta").mode("append").saveAsTable("catalog.schema.employees")

# read
df = spark.table("catalog.schema.employees")
```

### DeltaTable API

```python
from delta.tables import DeltaTable

# reference existing table
dt = DeltaTable.forName(spark, "catalog.schema.employees")
dt = DeltaTable.forPath(spark, "/mnt/delta/employees")

# check if path is Delta
DeltaTable.isDeltaTable(spark, "/mnt/delta/employees")   # True/False

# get history
dt.history().show()
dt.history(10).show()     # last 10 versions

# get details
dt.detail().show()

# convert Parquet to Delta
DeltaTable.convertToDelta(spark, "parquet.`/mnt/data/old_parquet_table`")

# delete
dt.delete(F.col("status") == "deleted")

# update
dt.update(
    condition=F.col("department") == "Engineering",
    set={"salary": F.col("salary") * 1.10}
)
```

---

## 33. MERGE — Upserts and SCD

MERGE is the most powerful Delta Lake operation — it handles insert-if-new, update-if-changed, and delete-if-missing in a single atomic transaction.

### Basic Upsert

```python
from delta.tables import DeltaTable
from pyspark.sql import functions as F

target = DeltaTable.forName(spark, "catalog.schema.employees")

target.alias("t").merge(
    source=df_updates.alias("s"),
    condition="t.id = s.id"
).whenMatchedUpdate(set={
    "name": "s.name",
    "salary": "s.salary",
    "department": "s.department",
}).whenNotMatchedInsert(values={
    "id": "s.id",
    "name": "s.name",
    "salary": "s.salary",
    "department": "s.department",
    "hire_date": "s.hire_date",
}).execute()
```

### Upsert with Delete

```python
target.alias("t").merge(
    source=df_updates.alias("s"),
    condition="t.id = s.id"
).whenMatchedUpdateAll() \
 .whenNotMatchedInsertAll() \
 .whenNotMatchedBySourceDelete() \
 .execute()
```

### Conditional MERGE

```python
target.alias("t").merge(
    source=df_updates.alias("s"),
    condition="t.id = s.id"
).whenMatchedUpdate(
    condition="s.updated_at > t.updated_at",
    set={"name": "s.name", "salary": "s.salary", "updated_at": "s.updated_at"}
).whenNotMatchedInsert(
    condition="s.status = 'active'",
    values={"id": "s.id", "name": "s.name", "salary": "s.salary", "status": "s.status"}
).execute()
```

### SCD Type 2 (Slowly Changing Dimension)

```python
target = DeltaTable.forName(spark, "catalog.schema.customer_dim")

# close old records and insert new ones
target.alias("t").merge(
    source=df_updates.alias("s"),
    condition="t.customer_id = s.customer_id AND t.is_current = true"
).whenMatchedUpdate(
    condition="t.name != s.name OR t.city != s.city",
    set={
        "is_current": F.lit(False),
        "end_date": F.current_date(),
    }
).whenNotMatchedInsert(values={
    "customer_id": "s.customer_id",
    "name": "s.name",
    "city": "s.city",
    "start_date": F.current_date(),
    "end_date": F.lit(None).cast("date"),
    "is_current": F.lit(True),
}).execute()

# insert updated version as new current record
changed = df_updates.alias("s").join(
    spark.table("catalog.schema.customer_dim").filter(F.col("is_current") == False).alias("t"),
    (F.col("s.customer_id") == F.col("t.customer_id")) & (F.col("t.end_date") == F.current_date()),
    "inner"
).select(
    F.col("s.customer_id"), F.col("s.name"), F.col("s.city"),
    F.current_date().alias("start_date"),
    F.lit(None).cast("date").alias("end_date"),
    F.lit(True).alias("is_current"),
)
changed.write.format("delta").mode("append").saveAsTable("catalog.schema.customer_dim")
```

### SQL MERGE

```sql
MERGE INTO catalog.schema.employees AS t
USING staging.new_employees AS s
ON t.id = s.id
WHEN MATCHED AND s.updated_at > t.updated_at THEN
    UPDATE SET t.name = s.name, t.salary = s.salary, t.updated_at = s.updated_at
WHEN NOT MATCHED THEN
    INSERT (id, name, salary, department, hire_date)
    VALUES (s.id, s.name, s.salary, s.department, s.hire_date)
WHEN NOT MATCHED BY SOURCE THEN
    DELETE;
```

---

## 34. Time Travel and Versioning

```python
# read a specific version
df_v5 = spark.read.format("delta").option("versionAsOf", 5).load("/mnt/delta/employees")
df_v5 = spark.read.format("delta").option("versionAsOf", 5).table("catalog.schema.employees")

# read as of a timestamp
df_old = spark.read.format("delta") \
    .option("timestampAsOf", "2024-03-15 10:00:00") \
    .table("catalog.schema.employees")

# view history
from delta.tables import DeltaTable
dt = DeltaTable.forName(spark, "catalog.schema.employees")
dt.history().show(truncate=False)
dt.history(10).select("version", "timestamp", "operation", "operationMetrics").show()
```

```sql
-- SQL time travel
SELECT * FROM catalog.schema.employees VERSION AS OF 5;
SELECT * FROM catalog.schema.employees TIMESTAMP AS OF '2024-03-15 10:00:00';

-- view changes between versions
DESCRIBE HISTORY catalog.schema.employees;
```

### Restore a Previous Version

```sql
RESTORE TABLE catalog.schema.employees TO VERSION AS OF 5;
RESTORE TABLE catalog.schema.employees TO TIMESTAMP AS OF '2024-03-15';
```

```python
dt.restoreToVersion(5)
dt.restoreToTimestamp("2024-03-15")
```

---

## 35. OPTIMIZE, VACUUM, and Maintenance

### OPTIMIZE — Compact Small Files

```sql
-- compact small files (improves read performance)
OPTIMIZE catalog.schema.orders;

-- optimize specific partitions
OPTIMIZE catalog.schema.orders WHERE order_date >= '2024-01-01';

-- Z-ORDER (co-locate frequently filtered columns)
OPTIMIZE catalog.schema.orders ZORDER BY (customer_id, order_date);
```

### VACUUM — Remove Old Files

```sql
-- delete files older than 7 days (default retention)
VACUUM catalog.schema.orders;

-- custom retention
VACUUM catalog.schema.orders RETAIN 30 HOURS;

-- dry run (see what would be deleted)
VACUUM catalog.schema.orders DRY RUN;
```

```python
dt = DeltaTable.forName(spark, "catalog.schema.orders")
dt.optimize().executeCompaction()
dt.optimize().where("order_date >= '2024-01-01'").executeZOrderBy("customer_id")
dt.vacuum(168)    # 168 hours = 7 days
```

### ANALYZE TABLE — Compute Statistics

```sql
-- compute stats for query optimizer
ANALYZE TABLE catalog.schema.orders COMPUTE STATISTICS;
ANALYZE TABLE catalog.schema.orders COMPUTE STATISTICS FOR COLUMNS customer_id, amount;
```

---

## 36. Schema Evolution and Enforcement

### Schema Enforcement (Default)

Delta rejects writes with mismatched schemas by default.

```python
# this will FAIL if df_new has extra columns
df_new.write.format("delta").mode("append").saveAsTable("catalog.schema.employees")
```

### Schema Evolution

```python
# allow new columns to be added automatically
df_new.write.format("delta") \
    .mode("append") \
    .option("mergeSchema", "true") \
    .saveAsTable("catalog.schema.employees")

# overwrite with new schema
df_new.write.format("delta") \
    .mode("overwrite") \
    .option("overwriteSchema", "true") \
    .saveAsTable("catalog.schema.employees")
```

```sql
-- enable auto merge for the table
ALTER TABLE catalog.schema.employees SET TBLPROPERTIES ('delta.autoMerge' = 'true');

-- add column
ALTER TABLE catalog.schema.employees ADD COLUMN (email STRING, phone STRING);

-- rename column
ALTER TABLE catalog.schema.employees RENAME COLUMN dept TO department;

-- change column type (only widening: int → long, float → double)
ALTER TABLE catalog.schema.employees ALTER COLUMN salary TYPE DOUBLE;

-- add comment
ALTER TABLE catalog.schema.employees ALTER COLUMN salary COMMENT 'Annual base salary in USD';

-- drop column (requires column mapping)
ALTER TABLE catalog.schema.employees SET TBLPROPERTIES (
    'delta.columnMapping.mode' = 'name',
    'delta.minReaderVersion' = '2',
    'delta.minWriterVersion' = '5'
);
ALTER TABLE catalog.schema.employees DROP COLUMN temp_col;
```

---

## 37. Change Data Feed (CDF)

Track row-level changes (inserts, updates, deletes).

```sql
-- enable CDF on a table
ALTER TABLE catalog.schema.employees SET TBLPROPERTIES (delta.enableChangeDataFeed = true);
```

```python
# read changes between versions
changes = spark.read.format("delta") \
    .option("readChangeFeed", "true") \
    .option("startingVersion", 5) \
    .option("endingVersion", 10) \
    .table("catalog.schema.employees")

changes.show()
# columns include: _change_type (insert, update_preimage, update_postimage, delete),
#                  _commit_version, _commit_timestamp

# read changes from a timestamp
changes = spark.read.format("delta") \
    .option("readChangeFeed", "true") \
    .option("startingTimestamp", "2024-03-15") \
    .table("catalog.schema.employees")

# filter by change type
inserts = changes.filter(F.col("_change_type") == "insert")
updates = changes.filter(F.col("_change_type") == "update_postimage")
deletes = changes.filter(F.col("_change_type") == "delete")
```

---

# Part 6: Data Engineering Patterns

---

## 38. Bronze-Silver-Gold (Medallion Architecture)

The standard Databricks data engineering pattern:

| Layer | Purpose | Data Quality | Format |
|-------|---------|-------------|--------|
| **Bronze** | Raw ingestion | As-is from source, append-only | Delta |
| **Silver** | Cleaned, conformed | Deduplicated, typed, validated | Delta |
| **Gold** | Business-level aggregates | Aggregated, joined, ready to query | Delta |

```python
# --- Bronze: raw ingestion ---
raw_df = spark.read.csv("/mnt/landing/sales/", header=True, inferSchema=True)
raw_df = raw_df.withColumn("_ingestion_timestamp", F.current_timestamp())
raw_df = raw_df.withColumn("_source_file", F.input_file_name())
raw_df.write.format("delta").mode("append").saveAsTable("catalog.bronze.sales_raw")

# --- Silver: cleaned ---
bronze_df = spark.table("catalog.bronze.sales_raw")

silver_df = (
    bronze_df
    # clean column names
    .toDF(*[c.lower().replace(" ", "_") for c in bronze_df.columns])
    # cast types
    .withColumn("amount", F.col("amount").cast("double"))
    .withColumn("order_date", F.to_date("order_date", "yyyy-MM-dd"))
    # remove nulls in required fields
    .filter(F.col("order_id").isNotNull())
    # deduplicate
    .dropDuplicates(["order_id"])
    # add metadata
    .withColumn("_processed_timestamp", F.current_timestamp())
)

silver_df.write.format("delta").mode("overwrite").saveAsTable("catalog.silver.sales_clean")

# --- Gold: business aggregates ---
silver_df = spark.table("catalog.silver.sales_clean")

gold_df = silver_df.groupBy(
    F.date_trunc("month", "order_date").alias("month"),
    "product_category",
).agg(
    F.sum("amount").alias("total_revenue"),
    F.count("order_id").alias("order_count"),
    F.countDistinct("customer_id").alias("unique_customers"),
    F.avg("amount").alias("avg_order_value"),
)

gold_df.write.format("delta").mode("overwrite").saveAsTable("catalog.gold.monthly_sales_summary")
```

---

## 39. Incremental Data Loading

### Watermark Pattern

```python
# get the latest processed timestamp
try:
    max_processed = spark.sql(
        "SELECT MAX(order_date) FROM catalog.silver.orders"
    ).collect()[0][0]
except:
    max_processed = "1900-01-01"

# load only new data
new_data = spark.table("catalog.bronze.orders_raw") \
    .filter(F.col("order_date") > max_processed)

if new_data.count() > 0:
    new_data.write.format("delta").mode("append").saveAsTable("catalog.silver.orders")
    print(f"Loaded {new_data.count()} new rows")
else:
    print("No new data")
```

### Incremental MERGE

```python
from delta.tables import DeltaTable

target = DeltaTable.forName(spark, "catalog.silver.customers")
source = spark.table("catalog.bronze.customers_raw").filter(F.col("_ingestion_timestamp") > max_ts)

target.alias("t").merge(
    source.alias("s"),
    "t.customer_id = s.customer_id"
).whenMatchedUpdateAll() \
 .whenNotMatchedInsertAll() \
 .execute()
```

---

## 40. Auto Loader — Streaming File Ingestion

Auto Loader automatically discovers and processes new files as they arrive.

```python
# basic Auto Loader
df = (
    spark.readStream
    .format("cloudFiles")
    .option("cloudFiles.format", "csv")
    .option("cloudFiles.schemaLocation", "/mnt/checkpoints/sales_schema")
    .option("header", "true")
    .option("cloudFiles.inferColumnTypes", "true")
    .load("/mnt/landing/sales/")
)

# write as streaming Delta
df.writeStream \
    .format("delta") \
    .option("checkpointLocation", "/mnt/checkpoints/sales") \
    .outputMode("append") \
    .trigger(availableNow=True) \
    .toTable("catalog.bronze.sales_raw")

# with transformations
df = (
    spark.readStream
    .format("cloudFiles")
    .option("cloudFiles.format", "json")
    .option("cloudFiles.schemaLocation", "/mnt/checkpoints/events_schema")
    .load("/mnt/landing/events/")
    .withColumn("_ingestion_time", F.current_timestamp())
    .withColumn("_source_file", F.input_file_name())
)

df.writeStream \
    .format("delta") \
    .option("checkpointLocation", "/mnt/checkpoints/events") \
    .trigger(availableNow=True) \
    .toTable("catalog.bronze.events_raw")
```

### Auto Loader Options

| Option | Description |
|--------|-------------|
| `cloudFiles.format` | csv, json, parquet, avro, text, binaryFile |
| `cloudFiles.schemaLocation` | Path to store inferred schema |
| `cloudFiles.inferColumnTypes` | Infer types beyond string (default: false) |
| `cloudFiles.schemaHints` | Override inferred types: `"id INT, date DATE"` |
| `cloudFiles.maxFilesPerTrigger` | Max files per micro-batch |

---

## 41. Structured Streaming

```python
# read stream from Delta table
stream_df = spark.readStream.format("delta").table("catalog.bronze.events")

# transformations
processed = (
    stream_df
    .withColumn("event_hour", F.date_trunc("hour", "event_timestamp"))
    .groupBy("event_hour", "event_type")
    .count()
)

# write stream
query = (
    processed.writeStream
    .format("delta")
    .outputMode("complete")
    .option("checkpointLocation", "/mnt/checkpoints/event_counts")
    .trigger(processingTime="1 minute")
    .toTable("catalog.silver.event_counts")
)

# check stream status
query.status
query.lastProgress
query.isActive

# stop stream
query.stop()

# trigger options:
# .trigger(processingTime="10 seconds")    # micro-batch every 10s
# .trigger(once=True)                      # one batch then stop (deprecated)
# .trigger(availableNow=True)              # process all available, then stop
# .trigger(continuous="1 second")          # continuous processing
```

---

## 42. Delta Live Tables (DLT)

DLT is a declarative framework for building production pipelines. Defined in notebooks but run as managed pipelines.

```python
import dlt
from pyspark.sql import functions as F

# --- Bronze ---
@dlt.table(comment="Raw sales data ingested from landing zone")
def sales_bronze():
    return (
        spark.readStream
        .format("cloudFiles")
        .option("cloudFiles.format", "csv")
        .option("header", "true")
        .load("/mnt/landing/sales/")
    )

# --- Silver (with expectations / data quality) ---
@dlt.table(comment="Cleaned sales data")
@dlt.expect("valid_amount", "amount > 0")
@dlt.expect_or_drop("valid_order_id", "order_id IS NOT NULL")
@dlt.expect_or_fail("valid_date", "order_date IS NOT NULL")
def sales_silver():
    return (
        dlt.read_stream("sales_bronze")
        .withColumn("amount", F.col("amount").cast("double"))
        .withColumn("order_date", F.to_date("order_date"))
        .dropDuplicates(["order_id"])
    )

# --- Gold ---
@dlt.table(comment="Monthly sales summary")
def sales_gold():
    return (
        dlt.read("sales_silver")
        .groupBy(F.date_trunc("month", "order_date").alias("month"))
        .agg(
            F.sum("amount").alias("total_revenue"),
            F.count("order_id").alias("total_orders"),
        )
    )
```

### DLT Expectations

| Decorator | Behavior on Violation |
|-----------|----------------------|
| `@dlt.expect("name", "condition")` | Log warning, keep row |
| `@dlt.expect_or_drop("name", "condition")` | Silently drop row |
| `@dlt.expect_or_fail("name", "condition")` | Fail the pipeline |

---

## 43. Orchestration with Databricks Workflows

Databricks Workflows (Jobs) allow you to schedule and chain notebooks and tasks.

```python
# --- task values: pass data between tasks ---

# in Task 1: set a value
dbutils.jobs.taskValues.set(key="row_count", value=df.count())
dbutils.jobs.taskValues.set(key="status", value="success")

# in Task 2: get the value
row_count = dbutils.jobs.taskValues.get(taskKey="task_1", key="row_count")
status = dbutils.jobs.taskValues.get(taskKey="task_1", key="status", default="unknown")
```

### Workflow Patterns

```python
# notebook as a reusable task
# Parameters come from widgets
env = dbutils.widgets.get("environment")
date = dbutils.widgets.get("processing_date")

try:
    # run pipeline
    df = extract(env, date)
    df = transform(df)
    load(df, env)
    dbutils.jobs.taskValues.set(key="status", value="success")
    dbutils.notebook.exit("SUCCESS")
except Exception as e:
    dbutils.jobs.taskValues.set(key="status", value="failed")
    dbutils.jobs.taskValues.set(key="error", value=str(e))
    raise
```

---

# Part 7: Performance and Best Practices

---

## 44. Partitioning Strategies

```python
# write with partitions
df.write.format("delta") \
    .partitionBy("year", "month") \
    .mode("overwrite") \
    .saveAsTable("catalog.schema.orders")

# repartition before write (control file count)
df.repartition(10).write.format("delta").mode("overwrite").save("/mnt/delta/data")

# repartition by column (for better join/groupby performance)
df = df.repartition("department")
df = df.repartition(20, "department")

# coalesce (reduce partitions without shuffle — merge only)
df = df.coalesce(1)     # single file output
df = df.coalesce(10)    # reduce from many to 10
```

### Partition Guidelines

| Guideline | Rule |
|-----------|------|
| Partition column cardinality | Low cardinality (< 10,000 distinct values) |
| Partition file size | Target 128 MB - 1 GB per partition file |
| Common partition columns | date, year/month, region, status |
| Don't partition on | High-cardinality columns (user_id, order_id) |
| Small tables (< 1 GB) | Don't partition at all |

---

## 45. Z-Ordering and Liquid Clustering

### Z-Ordering

Co-locates related data within Delta files for faster filtered reads.

```sql
-- Z-ORDER on columns you frequently filter on
OPTIMIZE catalog.schema.orders ZORDER BY (customer_id, product_category);
```

### Liquid Clustering (Databricks Runtime 13.3+)

Replaces partitioning and Z-ordering with automatic, incremental clustering.

```sql
-- create with liquid clustering
CREATE TABLE catalog.schema.orders (
    order_id INT, customer_id INT, amount DOUBLE, order_date DATE
)
USING DELTA
CLUSTER BY (customer_id, order_date);

-- change clustering columns (no rewrite needed)
ALTER TABLE catalog.schema.orders CLUSTER BY (order_date, product_category);

-- trigger clustering
OPTIMIZE catalog.schema.orders;
```

### When to Use What

| Strategy | Use When |
|----------|----------|
| No partitioning | Table < 1 GB |
| Partitioning | Always filter by one low-cardinality column (e.g., date) |
| Z-Ordering | Filter by multiple columns, medium-sized tables |
| Liquid Clustering | New tables, flexible query patterns, replace both partitioning + Z-ordering |

---

## 46. Caching and Persistence

```python
# cache (stores in memory)
df.cache()
df.count()          # triggers caching

# unpersist
df.unpersist()

# persist with storage level
from pyspark import StorageLevel
df.persist(StorageLevel.MEMORY_AND_DISK)
df.persist(StorageLevel.DISK_ONLY)

# Delta cache (automatic in Databricks — caches remote data on local SSD)
# no code needed, enabled by default on Delta tables
```

```sql
-- SQL cache
CACHE TABLE catalog.schema.lookup_table;
UNCACHE TABLE catalog.schema.lookup_table;

-- check what's cached
SHOW TABLES FROM global_temp;
```

### When to Cache

- DataFrames used multiple times in the same notebook
- Lookup tables used in multiple joins
- Intermediate results in iterative algorithms

### When NOT to Cache

- Data used only once
- Very large DataFrames (will spill to disk)
- Streaming DataFrames

---

## 47. Broadcast Joins and Skew Handling

### Broadcast Join

Force the smaller table to be sent to all executors (avoids shuffle).

```python
from pyspark.sql.functions import broadcast

# explicitly broadcast small table
result = df_large.join(broadcast(df_small), on="key", how="inner")
```

```python
# configure auto-broadcast threshold (default: 10MB)
spark.conf.set("spark.sql.autoBroadcastJoinThreshold", "50m")   # increase to 50MB
spark.conf.set("spark.sql.autoBroadcastJoinThreshold", "-1")    # disable auto-broadcast
```

### Skew Handling

Data skew occurs when some partitions are much larger than others.

```python
# enable adaptive query execution (handles skew automatically)
spark.conf.set("spark.sql.adaptive.enabled", "true")                    # default: true
spark.conf.set("spark.sql.adaptive.skewJoin.enabled", "true")          # default: true

# salting technique (manual skew fix)
from pyspark.sql.functions import rand, floor

salt_buckets = 10
df_skewed = df_skewed.withColumn("salt", (F.rand() * salt_buckets).cast("int"))
df_dimension = df_dimension.crossJoin(
    spark.range(salt_buckets).withColumnRenamed("id", "salt")
)
result = df_skewed.join(df_dimension, on=["key", "salt"], how="inner").drop("salt")
```

---

## 48. Explain Plans and Debugging

```python
# view logical and physical plan
df.explain()              # physical plan only
df.explain(True)          # parsed → analyzed → optimized → physical
df.explain("extended")    # same as True
df.explain("formatted")   # readable format
df.explain("cost")        # with cost estimates

# Spark UI (accessible from cluster page)
# look at:
#   - Stages: check for skew (uneven task durations)
#   - SQL tab: visual query plan
#   - Storage: cached DataFrames
#   - Executors: memory and disk usage
```

### Common Performance Red Flags

| Issue | Sign | Fix |
|-------|------|-----|
| Shuffle | `Exchange` in plan, slow stages | Broadcast small tables, repartition |
| Data skew | One task takes much longer | AQE, salting, pre-filtering |
| Small files | Many tiny files | OPTIMIZE, Auto Optimize |
| Full scan | Reading all data for simple query | Partition pruning, Z-Order |
| UDF bottleneck | Python UDF in plan | Replace with built-in functions |
| Collect to driver | `collect()` on large data | Use `show()`, `display()`, or `limit()` |

---

## 49. Common Errors and Fixes

### AnalysisException: Table or view not found

```python
# check the table exists
spark.catalog.tableExists("catalog.schema.table_name")

# check you're using the right catalog
spark.sql("SELECT current_catalog(), current_database()")

# set default catalog/schema
spark.sql("USE CATALOG my_catalog")
spark.sql("USE SCHEMA my_schema")
```

### Column ambiguity in joins

```python
# WRONG: ambiguous "id" after join
result = df1.join(df2, df1["id"] == df2["id"])
result.select("id")   # which id?

# FIX: use aliases
result = df1.alias("a").join(df2.alias("b"), F.col("a.id") == F.col("b.id"))
result.select("a.id", "b.name")

# or drop duplicate column
result = df1.join(df2, on="id", how="inner")   # single "id" when using on=
```

### Schema mismatch on write

```python
# enable schema evolution
df.write.option("mergeSchema", "true").mode("append").saveAsTable("table")

# or overwrite the schema
df.write.option("overwriteSchema", "true").mode("overwrite").saveAsTable("table")
```

### Out of memory

```python
# increase driver memory in cluster config
# spark.driver.memory = 16g

# don't collect large DataFrames
df.limit(1000).toPandas()     # instead of df.toPandas()

# repartition to spread data
df = df.repartition(200)

# persist to disk if reusing
df.persist(StorageLevel.DISK_ONLY)
```

### Slow UDFs

```python
# replace Python UDF with built-in Spark functions
# BAD
@udf(StringType())
def upper_name(name):
    return name.upper() if name else None

# GOOD (100x faster)
df = df.withColumn("name_upper", F.upper("name"))

# if UDF is unavoidable, use Pandas UDF
@pandas_udf(StringType())
def clean_text(s: pd.Series) -> pd.Series:
    return s.str.strip().str.lower()
```

---

# Part 8: Utilities and Productivity

---

## 50. File System Operations (dbutils.fs)

```python
# list files
dbutils.fs.ls("/mnt/datalake/raw/")
display(dbutils.fs.ls("/mnt/datalake/raw/"))

# list with details
for f in dbutils.fs.ls("/mnt/datalake/raw/"):
    print(f"{f.name:40s} {f.size:>15,} bytes")

# read a file
content = dbutils.fs.head("/mnt/datalake/config.json", 1000)   # first 1000 bytes

# copy
dbutils.fs.cp("/mnt/source/file.csv", "/mnt/dest/file.csv")
dbutils.fs.cp("/mnt/source/dir/", "/mnt/dest/dir/", recurse=True)

# move
dbutils.fs.mv("/mnt/source/file.csv", "/mnt/dest/file.csv")

# delete
dbutils.fs.rm("/mnt/temp/old_file.csv")
dbutils.fs.rm("/mnt/temp/old_dir/", recurse=True)

# create directory
dbutils.fs.mkdirs("/mnt/datalake/processed/2024/03/")

# write small file
dbutils.fs.put("/mnt/datalake/status.txt", "Pipeline completed successfully", overwrite=True)

# --- paths ---
# /mnt/...        → mounted storage
# dbfs:/...       → Databricks File System
# /Volumes/...    → Unity Catalog Volumes
# abfss://...     → Azure ADLS direct
# s3://...        → AWS S3 direct
```

```python
# %fs magic (shortcut)
%fs ls /mnt/datalake/raw/
%fs head /mnt/datalake/config.json
```

---

## 51. Secrets Management

```python
# list secret scopes
dbutils.secrets.listScopes()

# list secrets in a scope
dbutils.secrets.list("my-scope")

# get a secret value (never displayed in notebook output)
db_password = dbutils.secrets.get(scope="my-scope", key="db-password")
api_key = dbutils.secrets.get(scope="my-scope", key="api-key")

# use in connection strings
jdbc_url = f"jdbc:postgresql://host:5432/mydb"
properties = {
    "user": dbutils.secrets.get("my-scope", "db-user"),
    "password": dbutils.secrets.get("my-scope", "db-password"),
}
df = spark.read.jdbc(jdbc_url, "public.employees", properties=properties)
```

---

## 52. Connecting to External Systems

### Azure SQL / SQL Server

```python
jdbc_url = "jdbc:sqlserver://server.database.windows.net:1433;database=mydb"
df = spark.read.format("jdbc") \
    .option("url", jdbc_url) \
    .option("dbtable", "dbo.customers") \
    .option("user", dbutils.secrets.get("scope", "sql-user")) \
    .option("password", dbutils.secrets.get("scope", "sql-pass")) \
    .load()
```

### Azure Storage (ADLS Gen2)

```python
# using service principal
spark.conf.set(f"fs.azure.account.auth.type.{storage_account}.dfs.core.windows.net", "OAuth")
spark.conf.set(f"fs.azure.account.oauth.provider.type.{storage_account}.dfs.core.windows.net",
               "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
spark.conf.set(f"fs.azure.account.oauth2.client.id.{storage_account}.dfs.core.windows.net", client_id)
spark.conf.set(f"fs.azure.account.oauth2.client.secret.{storage_account}.dfs.core.windows.net",
               dbutils.secrets.get("scope", "sp-secret"))
spark.conf.set(f"fs.azure.account.oauth2.client.endpoint.{storage_account}.dfs.core.windows.net",
               f"https://login.microsoftonline.com/{tenant_id}/oauth2/token")

df = spark.read.parquet(f"abfss://container@{storage_account}.dfs.core.windows.net/path/")
```

### AWS S3

```python
spark.conf.set("spark.hadoop.fs.s3a.access.key", dbutils.secrets.get("scope", "aws-access-key"))
spark.conf.set("spark.hadoop.fs.s3a.secret.key", dbutils.secrets.get("scope", "aws-secret-key"))

df = spark.read.parquet("s3a://my-bucket/data/")
```

### REST APIs (using requests)

```python
import requests
import json

response = requests.get(
    "https://api.example.com/data",
    headers={"Authorization": f"Bearer {dbutils.secrets.get('scope', 'api-token')}"}
)
data = response.json()

df = spark.createDataFrame(data["results"])
```

---

## 53. Pandas on Spark (pandas API)

Use familiar Pandas syntax at Spark scale.

```python
import pyspark.pandas as ps

# read
psdf = ps.read_csv("/mnt/data/sales.csv")
psdf = ps.read_parquet("/mnt/data/sales.parquet")
psdf = spark.table("catalog.schema.sales").pandas_api()    # from Spark DataFrame

# familiar Pandas operations
psdf.head()
psdf.describe()
psdf.shape
psdf.dtypes
psdf.columns

# filtering
psdf[psdf["salary"] > 80000]
psdf[(psdf["dept"] == "Engineering") & (psdf["salary"] > 80000)]

# adding columns
psdf["annual"] = psdf["salary"] * 12
psdf["tier"] = psdf["salary"].apply(lambda x: "Senior" if x > 100000 else "Standard")

# groupby
psdf.groupby("department")["salary"].mean()
psdf.groupby("department").agg({"salary": ["mean", "max"], "name": "count"})

# merge
merged = ps.merge(df1, df2, on="customer_id", how="left")

# convert between Spark and Pandas API
spark_df = psdf.to_spark()
psdf = spark_df.pandas_api()
pandas_df = psdf.to_pandas()    # collects to driver — small data only
```

---

## 54. Visualization in Notebooks

### Built-in display()

```python
# display() renders tables with built-in charting
display(df)

# after running display(), click the chart icon (+) above the table to:
# - create bar, line, scatter, pie, map charts
# - customize axes, aggregations, grouping
# - download as PNG

# display with specific rows
display(df.limit(1000))
display(df.orderBy(F.desc("revenue")).limit(20))
```

### Matplotlib / Seaborn (with toPandas)

```python
import matplotlib.pyplot as plt
import seaborn as sns

# convert small dataset to Pandas
pdf = df.groupBy("department").agg(F.avg("salary").alias("avg_salary")).toPandas()

fig, ax = plt.subplots(figsize=(10, 6))
ax.bar(pdf["department"], pdf["avg_salary"])
ax.set_title("Average Salary by Department")
ax.set_ylabel("Salary ($)")
plt.xticks(rotation=45)
plt.tight_layout()
display(fig)    # use display() instead of plt.show() in Databricks
```

### Plotly

```python
import plotly.express as px

pdf = df.limit(10000).toPandas()
fig = px.scatter(pdf, x="experience", y="salary", color="department",
                 title="Salary vs Experience", hover_data=["name"])
fig.show()
```

---

## 55. Common Recipes and Patterns

### Profile a DataFrame

```python
def profile_df(df):
    """Quick profile of a DataFrame."""
    total = df.count()
    print(f"Rows: {total:,}")
    print(f"Columns: {len(df.columns)}")
    print(f"Partitions: {df.rdd.getNumPartitions()}")
    print()

    stats = []
    for c in df.columns:
        col_stats = df.agg(
            F.count(F.when(F.col(c).isNull(), 1)).alias("nulls"),
            F.countDistinct(c).alias("distinct"),
        ).collect()[0]

        stats.append({
            "column": c,
            "type": str(df.schema[c].dataType),
            "nulls": col_stats["nulls"],
            "null_pct": f"{col_stats['nulls'] / total * 100:.1f}%",
            "distinct": col_stats["distinct"],
        })

    display(spark.createDataFrame(stats))

profile_df(df)
```

### Compare Two DataFrames

```python
def compare_dfs(df1, df2, key_cols):
    """Find differences between two DataFrames."""
    # rows only in df1
    only_in_1 = df1.join(df2, on=key_cols, how="left_anti")
    print(f"Only in df1: {only_in_1.count()}")

    # rows only in df2
    only_in_2 = df2.join(df1, on=key_cols, how="left_anti")
    print(f"Only in df2: {only_in_2.count()}")

    # rows in both (can check for value differences)
    common = df1.alias("a").join(df2.alias("b"), on=key_cols, how="inner")
    print(f"In both: {common.count()}")

    return only_in_1, only_in_2, common

only1, only2, common = compare_dfs(df_old, df_new, ["id"])
```

### Flatten a Nested Schema

```python
def flatten_df(df, sep="_"):
    """Recursively flatten nested structs."""
    from pyspark.sql.types import StructType
    flat_cols = []
    for field in df.schema.fields:
        if isinstance(field.dataType, StructType):
            for sub in field.dataType.fields:
                flat_cols.append(F.col(f"{field.name}.{sub.name}").alias(f"{field.name}{sep}{sub.name}"))
        else:
            flat_cols.append(F.col(field.name))
    return df.select(flat_cols)

df_flat = flatten_df(df)
```

### Dynamic Column Selection

```python
# select columns matching a pattern
date_cols = [c for c in df.columns if "date" in c.lower() or "time" in c.lower()]
numeric_cols = [f.name for f in df.schema.fields if f.dataType in (IntegerType(), DoubleType(), LongType())]

# apply transformation to multiple columns
for col_name in ["price", "tax", "discount"]:
    df = df.withColumn(col_name, F.round(F.col(col_name), 2))

# bulk rename: strip prefix
df = df.select([F.col(c).alias(c.replace("src_", "")) for c in df.columns])
```

### SCD Type 1 (Overwrite with Latest)

```python
from delta.tables import DeltaTable

target = DeltaTable.forName(spark, "catalog.schema.customers")
target.alias("t").merge(
    df_new.alias("s"),
    "t.customer_id = s.customer_id"
).whenMatchedUpdateAll() \
 .whenNotMatchedInsertAll() \
 .execute()
```

### Row-Level Audit Columns

```python
df = (
    df
    .withColumn("_created_at", F.current_timestamp())
    .withColumn("_created_by", F.lit(spark.conf.get("spark.databricks.clusterUsageTags.clusterOwnerOrgId", "unknown")))
    .withColumn("_source_file", F.input_file_name())
    .withColumn("_row_hash", F.sha2(F.concat_ws("||", *df.columns), 256))
)
```

### Data Quality Assertions

```python
def assert_no_nulls(df, columns):
    for col_name in columns:
        null_count = df.filter(F.col(col_name).isNull()).count()
        assert null_count == 0, f"Column '{col_name}' has {null_count} null values"
    print("All null checks passed")

def assert_unique(df, columns):
    total = df.count()
    distinct = df.dropDuplicates(columns).count()
    assert total == distinct, f"Duplicate rows found: {total - distinct} duplicates on {columns}"
    print("Uniqueness check passed")

def assert_not_empty(df):
    count = df.count()
    assert count > 0, "DataFrame is empty"
    print(f"Not empty: {count:,} rows")

# usage
assert_not_empty(df)
assert_no_nulls(df, ["id", "name", "email"])
assert_unique(df, ["id"])
```

### Import Cheat Sheet for Databricks

```python
# always available
from pyspark.sql import functions as F
from pyspark.sql.window import Window
from pyspark.sql.types import *
from delta.tables import DeltaTable

# optional
import pandas as pd
import numpy as np
from datetime import datetime, timedelta
from functools import reduce
import json
```

---

*End of reference. Organized by workflow: connect → read → inspect → transform → write → optimize → automate.*
