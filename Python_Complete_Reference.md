---
layout: default
title: Python Complete Reference
---

# Python Complete Reference for Data Professionals

A comprehensive reference covering Python for data analysts, data engineers, and data scientists — from file I/O and Pandas basics through advanced transformations, visualization, and machine learning patterns.

Every section explains the concept, shows the syntax, and includes practical examples.

---

## Table of Contents

### Part 1: Python Fundamentals for Data Work
1. [Essential Python Refresher](#1-essential-python-refresher)
2. [File I/O — Reading and Writing Files](#2-file-io--reading-and-writing-files)
3. [Working with Paths and Directories](#3-working-with-paths-and-directories)
4. [List, Dict, and Set Comprehensions](#4-list-dict-and-set-comprehensions)
5. [Lambda, Map, Filter, Reduce](#5-lambda-map-filter-reduce)
6. [Error Handling and Logging](#6-error-handling-and-logging)
7. [Working with Dates and Times](#7-working-with-dates-and-times)
8. [Regular Expressions](#8-regular-expressions)

### Part 2: Data Loading and I/O
9. [Reading CSV, Excel, JSON, Parquet](#9-reading-csv-excel-json-parquet)
10. [Reading from Databases](#10-reading-from-databases)
11. [Reading from APIs](#11-reading-from-apis)
12. [Writing and Exporting Data](#12-writing-and-exporting-data)

### Part 3: Pandas — Data Manipulation
13. [DataFrames and Series Basics](#13-dataframes-and-series-basics)
14. [Selecting and Filtering Data](#14-selecting-and-filtering-data)
15. [Adding, Renaming, and Dropping Columns](#15-adding-renaming-and-dropping-columns)
16. [Data Types and Type Conversion](#16-data-types-and-type-conversion)
17. [Handling Missing Data](#17-handling-missing-data)
18. [String Operations](#18-string-operations)
19. [Date and Time Operations in Pandas](#19-date-and-time-operations-in-pandas)
20. [Sorting and Ranking](#20-sorting-and-ranking)
21. [Grouping and Aggregation](#21-grouping-and-aggregation)
22. [Merging, Joining, and Concatenating](#22-merging-joining-and-concatenating)
23. [Pivot Tables and Reshaping](#23-pivot-tables-and-reshaping)
24. [Apply, Map, and Transform](#24-apply-map-and-transform)
25. [Window Functions (Rolling, Expanding, Shifting)](#25-window-functions-rolling-expanding-shifting)
26. [Duplicates and Deduplication](#26-duplicates-and-deduplication)
27. [Binning, Cutting, and Categorization](#27-binning-cutting-and-categorization)
28. [MultiIndex and Advanced Indexing](#28-multiindex-and-advanced-indexing)

### Part 4: Data Cleaning Patterns
29. [End-to-End Cleaning Pipeline](#29-end-to-end-cleaning-pipeline)
30. [Outlier Detection and Treatment](#30-outlier-detection-and-treatment)
31. [Data Validation and Quality Checks](#31-data-validation-and-quality-checks)

### Part 5: NumPy Essentials
32. [NumPy Arrays and Operations](#32-numpy-arrays-and-operations)
33. [Linear Algebra and Statistics with NumPy](#33-linear-algebra-and-statistics-with-numpy)

### Part 6: Data Visualization
34. [Matplotlib Fundamentals](#34-matplotlib-fundamentals)
35. [Seaborn for Statistical Plots](#35-seaborn-for-statistical-plots)
36. [Plotly for Interactive Visualizations](#36-plotly-for-interactive-visualizations)

### Part 7: Feature Engineering and ML Prep
37. [Feature Engineering Patterns](#37-feature-engineering-patterns)
38. [Encoding Categorical Variables](#38-encoding-categorical-variables)
39. [Scaling and Normalization](#39-scaling-and-normalization)
40. [Train-Test Split and Cross Validation](#40-train-test-split-and-cross-validation)

### Part 8: Machine Learning with Scikit-Learn
41. [Regression Models](#41-regression-models)
42. [Classification Models](#42-classification-models)
43. [Clustering](#43-clustering)
44. [Model Evaluation Metrics](#44-model-evaluation-metrics)

### Part 9: Big Data and Performance
45. [Pandas Performance Optimization](#45-pandas-performance-optimization)
46. [PySpark Essentials](#46-pyspark-essentials)
47. [Polars — Fast DataFrame Library](#47-polars--fast-dataframe-library)

### Part 10: Automation and Pipelines
48. [Scheduling and Automation](#48-scheduling-and-automation)
49. [Environment and Dependency Management](#49-environment-and-dependency-management)
50. [Common Recipes and One-Liners](#50-common-recipes-and-one-liners)

---

# Part 1: Python Fundamentals for Data Work

---

## 1. Essential Python Refresher

### Data Structures

```python
# --- Lists (ordered, mutable) ---
nums = [1, 2, 3, 4, 5]
nums.append(6)              # [1, 2, 3, 4, 5, 6]
nums.extend([7, 8])         # [1, 2, 3, 4, 5, 6, 7, 8]
nums.insert(0, 0)           # insert at index 0
nums.pop()                  # remove and return last item
nums.remove(3)              # remove first occurrence of 3
nums[1:4]                   # slice: [1, 2, 4] (start inclusive, end exclusive)
nums[-3:]                   # last 3 elements
nums[::2]                   # every 2nd element

# --- Tuples (ordered, immutable) ---
point = (10, 20)
x, y = point                # unpacking

# --- Dictionaries (key-value, ordered since 3.7) ---
person = {"name": "Alice", "age": 30, "role": "engineer"}
person["email"] = "alice@co.com"           # add key
person.get("phone", "N/A")                 # safe get with default
person.pop("age")                          # remove key
person.keys()                              # dict_keys(['name', 'role', 'email'])
person.values()                            # dict_values([...])
person.items()                             # dict_items([('name', 'Alice'), ...])
{**person, "dept": "Engineering"}          # merge dicts (Python 3.5+)
person | {"dept": "Engineering"}           # merge dicts (Python 3.9+)

# --- Sets (unordered, unique values) ---
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}
a & b                       # intersection: {3, 4}
a | b                       # union: {1, 2, 3, 4, 5, 6}
a - b                       # difference: {1, 2}
a ^ b                       # symmetric difference: {1, 2, 5, 6}
```

### String Formatting

```python
name, amount = "Alice", 1234.567

# f-strings (Python 3.6+) — preferred
f"Hello {name}, total: ${amount:,.2f}"      # "Hello Alice, total: $1,234.57"
f"{name!r}"                                  # "Hello 'Alice'" (repr)
f"{amount:.0f}"                              # "1235" (no decimals)
f"{name:>20}"                                # right-align in 20 chars
f"{name:<20}"                                # left-align
f"{42:08b}"                                  # "00101010" (binary, 8 digits)

# useful for data work
pct = 0.8567
f"{pct:.1%}"                                 # "85.7%"
```

### Control Flow

```python
# ternary expression
status = "high" if salary > 100000 else "low"

# for-else (else runs if loop completes without break)
for item in items:
    if item == target:
        break
else:
    print("not found")

# walrus operator (Python 3.8+) — assign and test in one
if (n := len(data)) > 100:
    print(f"Processing {n} records")

# match-case (Python 3.10+)
match status_code:
    case 200:
        handle_success()
    case 404:
        handle_not_found()
    case _:
        handle_error()
```

### Functions

```python
# default arguments
def load_data(path, sep=",", encoding="utf-8"):
    ...

# *args and **kwargs
def log(*messages, level="INFO", **metadata):
    for msg in messages:
        print(f"[{level}] {msg} | {metadata}")

log("started", "loading", level="DEBUG", source="api", rows=1000)

# type hints (documentation, not enforced at runtime)
def clean_column(df: pd.DataFrame, col: str) -> pd.DataFrame:
    ...

# generators (memory-efficient for large data)
def read_large_file(path):
    with open(path) as f:
        for line in f:
            yield line.strip()

for line in read_large_file("huge.csv"):
    process(line)
```

### Useful Built-ins

```python
# enumerate — index + value
for i, name in enumerate(["Alice", "Bob", "Carol"]):
    print(f"{i}: {name}")

# zip — pair elements from multiple iterables
names = ["Alice", "Bob"]
scores = [95, 87]
for name, score in zip(names, scores):
    print(f"{name}: {score}")
dict(zip(names, scores))    # {"Alice": 95, "Bob": 87}

# sorted with key
sorted(employees, key=lambda e: e["salary"], reverse=True)

# any / all
any(x > 100 for x in values)   # True if at least one > 100
all(x > 0 for x in values)     # True if every value > 0

# collections
from collections import Counter, defaultdict, OrderedDict

Counter(["a", "b", "a", "c", "a", "b"])   # Counter({'a': 3, 'b': 2, 'c': 1})

d = defaultdict(list)
d["fruits"].append("apple")    # no KeyError
```

---

## 2. File I/O — Reading and Writing Files

### Reading Text Files

```python
# read entire file
with open("data.txt", "r", encoding="utf-8") as f:
    content = f.read()

# read lines into a list
with open("data.txt") as f:
    lines = f.readlines()          # includes \n
    lines = [l.strip() for l in f] # without \n

# read line by line (memory efficient)
with open("large_file.txt") as f:
    for line in f:
        process(line.strip())
```

### Writing Text Files

```python
# write (overwrites)
with open("output.txt", "w", encoding="utf-8") as f:
    f.write("line 1\n")
    f.write("line 2\n")

# append
with open("log.txt", "a") as f:
    f.write(f"{datetime.now()}: event occurred\n")

# write multiple lines
lines = ["header", "row1", "row2"]
with open("output.txt", "w") as f:
    f.writelines(line + "\n" for line in lines)
```

### Reading/Writing CSV (without Pandas)

```python
import csv

# read CSV
with open("data.csv", newline="", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["name"], row["salary"])

# write CSV
with open("output.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=["name", "salary"])
    writer.writeheader()
    writer.writerow({"name": "Alice", "salary": 95000})

# read CSV as list of lists
with open("data.csv") as f:
    reader = csv.reader(f)
    header = next(reader)
    data = [row for row in reader]
```

### Reading/Writing JSON

```python
import json

# read JSON file
with open("config.json") as f:
    config = json.load(f)

# write JSON file
with open("output.json", "w") as f:
    json.dump(data, f, indent=2, default=str)   # default=str handles dates

# parse JSON string
data = json.loads('{"name": "Alice", "age": 30}')

# convert to JSON string
json_str = json.dumps(data, indent=2)

# handle nested JSON
nested = {"users": [{"name": "Alice", "scores": [90, 85]}]}
nested["users"][0]["scores"][1]   # 85
```

### Reading/Writing YAML

```python
import yaml

# read
with open("config.yaml") as f:
    config = yaml.safe_load(f)

# write
with open("output.yaml", "w") as f:
    yaml.dump(config, f, default_flow_style=False)
```

### Reading .env Files

```python
from dotenv import load_dotenv
import os

load_dotenv()   # loads .env file into environment
db_host = os.getenv("DB_HOST", "localhost")
db_port = int(os.getenv("DB_PORT", "5432"))
```

### Pickle (Python Object Serialization)

```python
import pickle

# save Python object
with open("model.pkl", "wb") as f:
    pickle.dump(trained_model, f)

# load Python object
with open("model.pkl", "rb") as f:
    model = pickle.load(f)

# joblib (better for NumPy arrays / ML models)
import joblib
joblib.dump(model, "model.joblib")
model = joblib.load("model.joblib")
```

---

## 3. Working with Paths and Directories

```python
from pathlib import Path
import os, shutil, glob

# --- pathlib (modern, preferred) ---
p = Path("data/raw/sales.csv")
p.name           # "sales.csv"
p.stem           # "sales"
p.suffix         # ".csv"
p.parent         # Path("data/raw")
p.exists()       # True/False
p.is_file()      # True/False
p.is_dir()       # True/False

# build paths safely (works cross-platform)
data_dir = Path("data") / "processed"
file_path = data_dir / "clean_sales.csv"

# create directories
data_dir.mkdir(parents=True, exist_ok=True)

# list files
list(Path("data").glob("*.csv"))                  # CSVs in data/
list(Path("data").rglob("*.csv"))                 # CSVs in data/ and subdirs
list(Path("data").glob("**/*.parquet"))            # all parquet files recursively

# read/write with pathlib
content = Path("config.json").read_text(encoding="utf-8")
Path("output.txt").write_text("hello", encoding="utf-8")

# --- os module ---
os.getcwd()                         # current directory
os.listdir("data")                  # list dir contents
os.path.join("data", "raw", "f.csv")  # "data/raw/f.csv"
os.path.getsize("data.csv")        # file size in bytes
os.rename("old.csv", "new.csv")     # rename file

# --- shutil ---
shutil.copy("src.csv", "dst.csv")           # copy file
shutil.copytree("src_dir", "dst_dir")       # copy directory
shutil.rmtree("temp_dir")                   # delete directory recursively
shutil.move("file.csv", "archive/file.csv") # move file

# --- glob ---
import glob
csv_files = glob.glob("data/**/*.csv", recursive=True)
```

---

## 4. List, Dict, and Set Comprehensions

```python
# --- list comprehensions ---
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]
flat = [item for sublist in nested_list for item in sublist]

# with function call
clean_names = [name.strip().lower() for name in raw_names]

# conditional expression
labels = ["high" if x > 100 else "low" for x in values]

# --- dict comprehensions ---
word_lengths = {word: len(word) for word in ["hello", "world"]}
filtered = {k: v for k, v in data.items() if v is not None}
inverted = {v: k for k, v in original.items()}

# from two lists
col_types = dict(zip(columns, types))

# --- set comprehensions ---
unique_domains = {email.split("@")[1] for email in emails}

# --- generator expressions (lazy, memory efficient) ---
total = sum(x**2 for x in range(1_000_000))   # no list created in memory
```

---

## 5. Lambda, Map, Filter, Reduce

```python
from functools import reduce

# --- lambda: anonymous functions ---
double = lambda x: x * 2
full_name = lambda first, last: f"{first} {last}"

# common use: sort key
sorted(people, key=lambda p: p["age"])
sorted(files, key=lambda f: f.stat().st_mtime)

# --- map: apply function to every element ---
names = list(map(str.upper, ["alice", "bob", "carol"]))
# same as: [name.upper() for name in ["alice", "bob", "carol"]]

nums = list(map(int, ["1", "2", "3"]))

# --- filter: keep elements where function returns True ---
adults = list(filter(lambda p: p["age"] >= 18, people))
# same as: [p for p in people if p["age"] >= 18]

non_empty = list(filter(None, ["a", "", "b", None, "c"]))  # ["a", "b", "c"]

# --- reduce: accumulate to a single value ---
total = reduce(lambda acc, x: acc + x, [1, 2, 3, 4, 5])  # 15
max_val = reduce(lambda a, b: a if a > b else b, [3, 1, 4, 1, 5])  # 5

# practical: flatten nested lists
nested = [[1, 2], [3, 4], [5]]
flat = reduce(lambda acc, lst: acc + lst, nested)  # [1, 2, 3, 4, 5]
```

**When to use:** List comprehensions are preferred for readability. Use `map`/`filter` when passing an existing function (like `str.upper`). Use `lambda` for simple one-off sort keys.

---

## 6. Error Handling and Logging

### Try / Except

```python
# basic
try:
    result = 10 / x
except ZeroDivisionError:
    result = 0

# multiple exceptions
try:
    data = json.loads(raw_text)
except (json.JSONDecodeError, TypeError) as e:
    print(f"Parse error: {e}")
    data = {}

# full pattern
try:
    df = pd.read_csv(path)
except FileNotFoundError:
    print(f"File not found: {path}")
    df = pd.DataFrame()
except pd.errors.EmptyDataError:
    print(f"Empty file: {path}")
    df = pd.DataFrame()
else:
    print(f"Loaded {len(df)} rows")       # runs only if no exception
finally:
    print("Cleanup complete")              # always runs

# re-raise with context
try:
    process(data)
except Exception as e:
    raise RuntimeError(f"Failed processing {filename}") from e
```

### Custom Exceptions

```python
class DataValidationError(Exception):
    def __init__(self, column, issue):
        self.column = column
        self.issue = issue
        super().__init__(f"Validation failed on '{column}': {issue}")

if df["age"].min() < 0:
    raise DataValidationError("age", "negative values found")
```

### Logging

```python
import logging

# basic setup
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(message)s",
    handlers=[
        logging.FileHandler("pipeline.log"),
        logging.StreamHandler()
    ]
)
logger = logging.getLogger(__name__)

# usage
logger.info(f"Loaded {len(df)} rows from {path}")
logger.warning(f"Column 'phone' has {null_pct:.1%} nulls")
logger.error(f"Failed to connect to database: {e}")
logger.debug(f"Query: {sql}")

# in a pipeline
def load_data(path):
    logger.info(f"Loading {path}")
    try:
        df = pd.read_csv(path)
        logger.info(f"Success: {len(df)} rows, {len(df.columns)} columns")
        return df
    except Exception as e:
        logger.error(f"Failed to load {path}: {e}")
        raise
```

---

## 7. Working with Dates and Times

```python
from datetime import datetime, date, timedelta, timezone
from dateutil import parser, relativedelta
import calendar

# --- creating dates ---
now = datetime.now()                          # local time
utc_now = datetime.now(timezone.utc)          # UTC
specific = datetime(2024, 3, 15, 14, 30, 0)  # 2024-03-15 14:30:00
today = date.today()                          # date only

# --- parsing strings ---
dt = datetime.strptime("2024-03-15", "%Y-%m-%d")
dt = datetime.strptime("15/03/2024 14:30", "%d/%m/%Y %H:%M")

# dateutil: handles many formats automatically
dt = parser.parse("March 15, 2024 2:30 PM")
dt = parser.parse("2024-03-15T14:30:00Z")

# --- formatting ---
now.strftime("%Y-%m-%d")          # "2024-03-15"
now.strftime("%B %d, %Y")        # "March 15, 2024"
now.strftime("%Y-%m-%d %H:%M")   # "2024-03-15 14:30"
now.isoformat()                   # "2024-03-15T14:30:00"

# --- arithmetic ---
tomorrow = today + timedelta(days=1)
last_week = now - timedelta(weeks=1)
duration = datetime(2024, 12, 31) - datetime(2024, 1, 1)
duration.days                      # 365

# dateutil relativedelta (handles months/years correctly)
from dateutil.relativedelta import relativedelta
next_month = now + relativedelta(months=1)
last_year = now - relativedelta(years=1)

# --- extracting components ---
now.year, now.month, now.day       # 2024, 3, 15
now.hour, now.minute, now.second   # 14, 30, 0
now.weekday()                      # 0=Monday, 6=Sunday
now.isoweekday()                   # 1=Monday, 7=Sunday
calendar.month_name[now.month]     # "March"
calendar.day_name[now.weekday()]   # "Friday"

# --- comparisons ---
if datetime(2024, 1, 1) < now < datetime(2025, 1, 1):
    print("It's 2024")

# --- timestamps ---
timestamp = now.timestamp()        # seconds since epoch (float)
datetime.fromtimestamp(timestamp)   # back to datetime
```

### Common Format Codes

| Code | Meaning | Example |
|------|---------|---------|
| `%Y` | 4-digit year | 2024 |
| `%m` | Zero-padded month | 03 |
| `%d` | Zero-padded day | 15 |
| `%H` | Hour (24h) | 14 |
| `%I` | Hour (12h) | 02 |
| `%M` | Minute | 30 |
| `%S` | Second | 00 |
| `%p` | AM/PM | PM |
| `%A` | Full weekday | Friday |
| `%B` | Full month | March |
| `%j` | Day of year | 075 |
| `%W` | Week number | 11 |

---

## 8. Regular Expressions

```python
import re

text = "Call 555-123-4567 or email alice@example.com by 2024-03-15"

# --- search: find first match ---
match = re.search(r"\d{3}-\d{3}-\d{4}", text)
if match:
    print(match.group())    # "555-123-4567"
    print(match.start())    # starting index

# --- findall: all matches ---
emails = re.findall(r"[\w.+-]+@[\w-]+\.[\w.]+", text)   # ["alice@example.com"]
dates = re.findall(r"\d{4}-\d{2}-\d{2}", text)          # ["2024-03-15"]
numbers = re.findall(r"\d+", "abc 123 def 456")          # ["123", "456"]

# --- sub: replace ---
cleaned = re.sub(r"\s+", " ", "  too   many   spaces  ")  # "too many spaces"
no_digits = re.sub(r"\d", "", "abc123def456")               # "abcdef"
redacted = re.sub(r"\d{3}-\d{3}-\d{4}", "[REDACTED]", text)

# --- split ---
parts = re.split(r"[,;|]+", "a,b;;c|d")   # ["a", "b", "c", "d"]

# --- groups: capture parts of the match ---
match = re.search(r"(\d{4})-(\d{2})-(\d{2})", text)
if match:
    year, month, day = match.groups()   # ("2024", "03", "15")

# named groups
pattern = r"(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})"
match = re.search(pattern, text)
match.group("year")    # "2024"

# --- compile for reuse ---
email_pattern = re.compile(r"[\w.+-]+@[\w-]+\.[\w.]+", re.IGNORECASE)
emails = email_pattern.findall(text)

# --- flags ---
re.IGNORECASE    # or re.I
re.MULTILINE     # or re.M — ^ and $ match line boundaries
re.DOTALL        # or re.S — . matches newlines
```

### Common Patterns for Data Work

```python
# validate email
re.match(r"^[\w.+-]+@[\w-]+\.[\w.]+$", email)

# extract numbers (including decimals and negatives)
re.findall(r"-?\d+\.?\d*", "price: $12.50, discount: -3.00")  # ["12.50", "-3.00"]

# clean column names
clean = re.sub(r"[^\w]", "_", "Revenue ($)").lower()   # "revenue____"
clean = re.sub(r"_+", "_", clean).strip("_")            # "revenue"

# parse log lines
log = '2024-03-15 14:30:00 ERROR [auth] Login failed for user=alice ip=10.0.0.1'
pattern = r"(?P<ts>[\d-]+ [\d:]+) (?P<level>\w+) \[(?P<module>\w+)\] (?P<msg>.+)"
match = re.match(pattern, log)
match.groupdict()  # {"ts": "2024-03-15 14:30:00", "level": "ERROR", ...}
```

---

# Part 2: Data Loading and I/O

---

## 9. Reading CSV, Excel, JSON, Parquet

```python
import pandas as pd

# ============================================================
# CSV
# ============================================================

# basic read
df = pd.read_csv("data.csv")

# common parameters
df = pd.read_csv(
    "data.csv",
    sep=",",                          # delimiter (use "\t" for TSV)
    header=0,                         # row number for header (None if no header)
    names=["col1", "col2", "col3"],   # custom column names
    usecols=["col1", "col3"],         # only load these columns
    dtype={"id": str, "amount": float},  # force types
    parse_dates=["date_col"],         # parse as datetime
    na_values=["NA", "N/A", "", "-"], # treat as NaN
    nrows=1000,                       # only first 1000 rows
    skiprows=5,                       # skip first 5 rows
    encoding="utf-8",                 # or "latin-1", "cp1252"
    low_memory=False,                 # avoid mixed-type warnings
)

# read in chunks (for large files)
chunks = pd.read_csv("huge.csv", chunksize=100_000)
for chunk in chunks:
    process(chunk)

# combine chunks
df = pd.concat(pd.read_csv("huge.csv", chunksize=100_000))

# read from URL
df = pd.read_csv("https://example.com/data.csv")

# read from string
from io import StringIO
df = pd.read_csv(StringIO("a,b\n1,2\n3,4"))

# ============================================================
# Excel
# ============================================================

df = pd.read_excel("report.xlsx")

df = pd.read_excel(
    "report.xlsx",
    sheet_name="Sheet2",              # by name
    sheet_name=0,                     # by index
    header=1,                         # header row
    usecols="A:D",                    # column range
    usecols=[0, 1, 3],               # by index
    skiprows=3,
    dtype={"id": str},
)

# read all sheets
all_sheets = pd.read_excel("report.xlsx", sheet_name=None)  # dict of DataFrames
for sheet_name, df in all_sheets.items():
    print(f"{sheet_name}: {len(df)} rows")

# read with openpyxl engine (default for .xlsx)
df = pd.read_excel("report.xlsx", engine="openpyxl")

# ============================================================
# JSON
# ============================================================

# standard JSON array of objects
df = pd.read_json("data.json")

# JSON lines (one JSON object per line) — common in data engineering
df = pd.read_json("data.jsonl", lines=True)

# nested JSON — normalize
import json
with open("nested.json") as f:
    data = json.load(f)

df = pd.json_normalize(
    data["results"],
    record_path="items",              # nested array to expand
    meta=["order_id", "customer"],    # parent fields to keep
    sep="_",                          # separator for nested key names
)

# ============================================================
# Parquet
# ============================================================

df = pd.read_parquet("data.parquet")

# read specific columns (very efficient — Parquet is columnar)
df = pd.read_parquet("data.parquet", columns=["id", "name", "revenue"])

# read from partitioned dataset
df = pd.read_parquet("data/year=2024/")

# with filters (pushdown — only reads matching row groups)
df = pd.read_parquet("data.parquet", filters=[("year", "==", 2024)])

# ============================================================
# Other formats
# ============================================================

# Feather (fast, Arrow-native)
df = pd.read_feather("data.feather")

# HDF5
df = pd.read_hdf("data.h5", key="dataset_name")

# Fixed-width
df = pd.read_fwf("data.txt", widths=[10, 20, 15])

# HTML tables from a webpage
tables = pd.read_html("https://en.wikipedia.org/wiki/List_of_countries")
df = tables[0]

# Clipboard
df = pd.read_clipboard()
```

---

## 10. Reading from Databases

### SQLAlchemy + Pandas

```python
from sqlalchemy import create_engine
import pandas as pd

# --- connection strings ---
# PostgreSQL
engine = create_engine("postgresql://user:pass@host:5432/dbname")

# MySQL
engine = create_engine("mysql+pymysql://user:pass@host:3306/dbname")

# SQL Server
engine = create_engine("mssql+pyodbc://user:pass@host/dbname?driver=ODBC+Driver+17+for+SQL+Server")

# SQLite
engine = create_engine("sqlite:///local.db")

# --- read with SQL query ---
df = pd.read_sql("SELECT * FROM employees WHERE salary > 70000", engine)

# --- read entire table ---
df = pd.read_sql_table("employees", engine)

# --- parameterized queries (safe from SQL injection) ---
from sqlalchemy import text
query = text("SELECT * FROM employees WHERE department = :dept AND salary > :min_sal")
df = pd.read_sql(query, engine, params={"dept": "Engineering", "min_sal": 70000})

# --- read in chunks ---
for chunk in pd.read_sql("SELECT * FROM big_table", engine, chunksize=50_000):
    process(chunk)

# --- write to database ---
df.to_sql("table_name", engine, if_exists="replace", index=False)
# if_exists: "fail" (default), "replace" (drop+create), "append"

df.to_sql("table_name", engine, if_exists="append", index=False,
          dtype={"date_col": sqlalchemy.Date()},
          method="multi",     # batch insert
          chunksize=5000)
```

### Direct Database Drivers

```python
# --- psycopg2 (PostgreSQL) ---
import psycopg2

conn = psycopg2.connect(host="localhost", dbname="mydb", user="user", password="pass")
cur = conn.cursor()
cur.execute("SELECT * FROM employees WHERE department = %s", ("Engineering",))
rows = cur.fetchall()
columns = [desc[0] for desc in cur.description]
df = pd.DataFrame(rows, columns=columns)
conn.close()

# --- sqlite3 ---
import sqlite3

conn = sqlite3.connect("local.db")
df = pd.read_sql_query("SELECT * FROM users", conn)
conn.close()

# --- BigQuery ---
from google.cloud import bigquery

client = bigquery.Client(project="my-project")
query = "SELECT * FROM `my-project.dataset.table` LIMIT 1000"
df = client.query(query).to_dataframe()
```

---

## 11. Reading from APIs

```python
import requests
import pandas as pd

# --- basic GET ---
response = requests.get("https://api.example.com/data")
response.raise_for_status()   # raises HTTPError for 4xx/5xx
data = response.json()
df = pd.DataFrame(data)

# --- with parameters ---
params = {"page": 1, "per_page": 100, "status": "active"}
response = requests.get("https://api.example.com/users", params=params)

# --- with headers / auth ---
headers = {"Authorization": "Bearer YOUR_TOKEN", "Content-Type": "application/json"}
response = requests.get("https://api.example.com/data", headers=headers)

# --- POST ---
payload = {"query": "SELECT * FROM events", "limit": 1000}
response = requests.post("https://api.example.com/query", json=payload, headers=headers)

# --- pagination loop ---
all_data = []
page = 1
while True:
    resp = requests.get(f"https://api.example.com/items?page={page}&per_page=100")
    resp.raise_for_status()
    items = resp.json()["data"]
    if not items:
        break
    all_data.extend(items)
    page += 1
df = pd.DataFrame(all_data)

# --- retry with backoff ---
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

session = requests.Session()
retries = Retry(total=3, backoff_factor=1, status_forcelist=[429, 500, 502, 503])
session.mount("https://", HTTPAdapter(max_retries=retries))
response = session.get("https://api.example.com/data")

# --- async requests (for multiple endpoints) ---
import asyncio
import aiohttp

async def fetch(session, url):
    async with session.get(url) as resp:
        return await resp.json()

async def fetch_all(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [fetch(session, url) for url in urls]
        return await asyncio.gather(*tasks)

results = asyncio.run(fetch_all(["https://api.com/1", "https://api.com/2"]))
```

---

## 12. Writing and Exporting Data

```python
import pandas as pd

# --- CSV ---
df.to_csv("output.csv", index=False)

df.to_csv(
    "output.csv",
    index=False,
    sep="|",
    encoding="utf-8-sig",       # BOM for Excel compatibility
    na_rep="NULL",               # represent NaN as
    columns=["col1", "col2"],    # only these columns
    date_format="%Y-%m-%d",
    float_format="%.2f",
    quoting=csv.QUOTE_NONNUMERIC,
)

# --- Excel ---
df.to_excel("output.xlsx", index=False, sheet_name="Results")

# multiple sheets
with pd.ExcelWriter("report.xlsx", engine="openpyxl") as writer:
    df_summary.to_excel(writer, sheet_name="Summary", index=False)
    df_detail.to_excel(writer, sheet_name="Details", index=False)
    df_raw.to_excel(writer, sheet_name="Raw Data", index=False)

# --- JSON ---
df.to_json("output.json", orient="records", indent=2)

# JSON lines
df.to_json("output.jsonl", orient="records", lines=True)

# --- Parquet ---
df.to_parquet("output.parquet", index=False, engine="pyarrow")

# with compression
df.to_parquet("output.parquet", compression="snappy")  # default
df.to_parquet("output.parquet", compression="gzip")     # smaller, slower

# partitioned parquet
df.to_parquet("output/", partition_cols=["year", "month"], index=False)

# --- Feather ---
df.to_feather("output.feather")

# --- Clipboard ---
df.to_clipboard(index=False)   # paste into Excel
```

---

# Part 3: Pandas — Data Manipulation

---

## 13. DataFrames and Series Basics

### Creating DataFrames

```python
import pandas as pd
import numpy as np

# from dictionary
df = pd.DataFrame({
    "name": ["Alice", "Bob", "Carol", "David"],
    "department": ["Engineering", "Sales", "Engineering", "HR"],
    "salary": [95000, 65000, 88000, 72000],
    "hire_date": pd.to_datetime(["2020-01-15", "2021-03-01", "2019-07-20", "2022-11-10"]),
})

# from list of dicts
df = pd.DataFrame([
    {"name": "Alice", "salary": 95000},
    {"name": "Bob", "salary": 65000},
])

# from NumPy array
df = pd.DataFrame(np.random.randn(5, 3), columns=["a", "b", "c"])

# empty DataFrame with schema
df = pd.DataFrame(columns=["id", "name", "value"])
```

### Inspecting Data

```python
df.head()              # first 5 rows
df.head(20)            # first 20 rows
df.tail()              # last 5 rows
df.sample(10)          # 10 random rows
df.shape               # (rows, columns)
len(df)                # number of rows
df.columns             # column names
df.dtypes              # data types per column
df.info()              # types, non-null counts, memory usage
df.describe()          # statistics for numeric columns
df.describe(include="all")  # include non-numeric
df.nunique()           # unique values per column
df.memory_usage(deep=True)  # memory in bytes per column

# value counts
df["department"].value_counts()              # counts per unique value
df["department"].value_counts(normalize=True) # as proportions
df["department"].value_counts(dropna=False)   # include NaN
```

### Series

A single column. Most DataFrame operations return or accept Series.

```python
s = df["salary"]           # select column → Series
type(s)                    # pandas.core.series.Series

s.mean()                   # 80000.0
s.median()                 # 80000.0
s.std()                    # standard deviation
s.min(), s.max()           # min and max
s.sum()                    # total
s.quantile(0.75)           # 75th percentile
s.unique()                 # unique values
s.nunique()                # count of unique values
s.is_unique                # True if all values unique
s.isin([65000, 95000])     # boolean mask
```

---

## 14. Selecting and Filtering Data

### Column Selection

```python
# single column → Series
df["name"]

# multiple columns → DataFrame
df[["name", "salary"]]

# dot notation (works for simple column names, no spaces)
df.name

# select by dtype
df.select_dtypes(include=["number"])
df.select_dtypes(include=["object"])         # string columns
df.select_dtypes(exclude=["datetime64"])
```

### Row Selection

```python
# by label (.loc) — label-based, inclusive on both ends
df.loc[0]                      # first row (if index is 0,1,2...)
df.loc[0:4]                    # rows 0 through 4 (inclusive!)
df.loc[0:4, "name":"salary"]   # rows 0-4, columns name through salary

# by position (.iloc) — integer-based, exclusive end
df.iloc[0]                     # first row
df.iloc[0:4]                   # rows 0 through 3 (exclusive end)
df.iloc[:, 0:3]                # all rows, first 3 columns
df.iloc[-1]                    # last row
df.iloc[::2]                   # every other row

# single value
df.loc[0, "name"]              # "Alice"
df.iloc[0, 1]                  # value at row 0, column 1
df.at[0, "name"]               # fast scalar access (label)
df.iat[0, 1]                   # fast scalar access (position)
```

### Boolean Filtering

```python
# single condition
df[df["salary"] > 80000]

# multiple conditions (use & for AND, | for OR, ~ for NOT)
df[(df["salary"] > 80000) & (df["department"] == "Engineering")]
df[(df["department"] == "Sales") | (df["department"] == "HR")]
df[~df["department"].isin(["HR", "Finance"])]

# string conditions
df[df["name"].str.contains("ali", case=False)]
df[df["name"].str.startswith("A")]
df[df["email"].str.endswith("@gmail.com")]

# date conditions
df[df["hire_date"] > "2021-01-01"]
df[df["hire_date"].between("2020-01-01", "2022-12-31")]
df[df["hire_date"].dt.year == 2021]

# null conditions
df[df["manager_id"].isna()]
df[df["manager_id"].notna()]

# isin
df[df["department"].isin(["Engineering", "Sales"])]

# query method (string-based, often cleaner for complex conditions)
df.query("salary > 80000 and department == 'Engineering'")
df.query("department in @dept_list")     # @ references Python variables
df.query("name.str.contains('Ali')", engine="python")
```

### Conditional Selection with np.where / np.select

```python
# np.where: if-else
df["level"] = np.where(df["salary"] > 80000, "Senior", "Junior")

# np.select: multiple conditions
conditions = [
    df["salary"] >= 100000,
    df["salary"] >= 70000,
    df["salary"] >= 40000,
]
choices = ["Senior", "Mid", "Junior"]
df["level"] = np.select(conditions, choices, default="Entry")
```

---

## 15. Adding, Renaming, and Dropping Columns

### Adding Columns

```python
# direct assignment
df["annual_salary"] = df["salary"] * 12
df["full_name"] = df["first_name"] + " " + df["last_name"]
df["is_senior"] = df["salary"] > 100000
df["bonus"] = df["salary"] * 0.10

# conditional column
df["tier"] = np.where(df["salary"] > 80000, "high", "standard")

# from a function
df["name_length"] = df["name"].apply(len)
df["email_domain"] = df["email"].apply(lambda x: x.split("@")[1])

# assign (returns new DataFrame, chainable)
df = df.assign(
    tax=df["salary"] * 0.3,
    net_salary=lambda x: x["salary"] - x["tax"],
)

# insert at specific position
df.insert(2, "country", "USA")    # insert at column index 2

# constant column
df["source"] = "file_a"
df["load_date"] = pd.Timestamp.now()
```

### Renaming Columns

```python
# rename specific columns
df = df.rename(columns={"old_name": "new_name", "amt": "amount"})

# rename all columns
df.columns = ["col1", "col2", "col3"]

# clean column names
df.columns = df.columns.str.lower()
df.columns = df.columns.str.replace(" ", "_")
df.columns = df.columns.str.strip()

# all at once: clean column names
df.columns = (
    df.columns
    .str.lower()
    .str.replace(r"[^\w]", "_", regex=True)
    .str.replace(r"_+", "_", regex=True)
    .str.strip("_")
)

# add prefix/suffix
df = df.add_prefix("raw_")
df = df.add_suffix("_v2")
```

### Dropping Columns

```python
# drop single column
df = df.drop(columns="temp_col")

# drop multiple columns
df = df.drop(columns=["temp1", "temp2", "debug"])

# drop by condition
cols_to_drop = [c for c in df.columns if c.startswith("unnamed")]
df = df.drop(columns=cols_to_drop)

# drop columns with too many nulls
threshold = 0.5
null_pct = df.isnull().mean()
df = df.drop(columns=null_pct[null_pct > threshold].index)

# keep only specific columns
df = df[["name", "salary", "department"]]

# drop rows (axis=0 is default)
df = df.drop(index=[0, 1, 2])         # by index
df = df.drop(df[df["salary"] < 0].index)  # by condition
```

### Reordering Columns

```python
# explicit order
df = df[["id", "name", "department", "salary", "hire_date"]]

# move a column to the front
col = "id"
df = df[[col] + [c for c in df.columns if c != col]]

# sort columns alphabetically
df = df.reindex(sorted(df.columns), axis=1)
```

---

## 16. Data Types and Type Conversion

```python
# check types
df.dtypes
df["col"].dtype

# --- converting types ---
df["id"] = df["id"].astype(int)
df["id"] = df["id"].astype(str)
df["price"] = df["price"].astype(float)
df["is_active"] = df["is_active"].astype(bool)

# categorical (saves memory for low-cardinality string columns)
df["department"] = df["department"].astype("category")

# nullable integer (allows NaN in integer columns)
df["age"] = df["age"].astype("Int64")        # capital I — nullable
df["flag"] = df["flag"].astype("boolean")    # nullable boolean

# numeric conversion with error handling
df["amount"] = pd.to_numeric(df["amount"], errors="coerce")   # invalid → NaN
df["amount"] = pd.to_numeric(df["amount"], errors="ignore")   # invalid → keep original

# datetime conversion
df["date"] = pd.to_datetime(df["date"])
df["date"] = pd.to_datetime(df["date"], format="%d/%m/%Y")
df["date"] = pd.to_datetime(df["date"], errors="coerce")      # invalid → NaT
df["date"] = pd.to_datetime(df["date"], unit="s")              # from Unix timestamp

# timedelta
df["duration"] = pd.to_timedelta(df["duration_str"])

# downcast for memory savings
df["amount"] = pd.to_numeric(df["amount"], downcast="float")   # float64 → float32
df["count"] = pd.to_numeric(df["count"], downcast="integer")   # int64 → int8/16/32
```

### Memory Optimization Pattern

```python
def optimize_dtypes(df):
    for col in df.select_dtypes(include=["int64"]).columns:
        df[col] = pd.to_numeric(df[col], downcast="integer")
    for col in df.select_dtypes(include=["float64"]).columns:
        df[col] = pd.to_numeric(df[col], downcast="float")
    for col in df.select_dtypes(include=["object"]).columns:
        if df[col].nunique() / len(df) < 0.5:
            df[col] = df[col].astype("category")
    return df

df = optimize_dtypes(df)
```

---

## 17. Handling Missing Data

### Detecting Missing Data

```python
df.isnull()              # boolean DataFrame
df.isna()                # same as isnull()
df.notna()               # inverse

df.isnull().sum()        # count NaN per column
df.isnull().mean()       # proportion of NaN per column
df.isnull().any()        # True if any NaN in column

# total nulls in entire DataFrame
df.isnull().sum().sum()

# rows with any null
df[df.isnull().any(axis=1)]

# columns with nulls
null_cols = df.columns[df.isnull().any()]

# null heatmap pattern
df.isnull().sum().sort_values(ascending=False).head(20)
```

### Dropping Missing Data

```python
# drop rows with ANY null
df_clean = df.dropna()

# drop rows where specific columns are null
df_clean = df.dropna(subset=["email", "phone"])

# drop rows where ALL values are null
df_clean = df.dropna(how="all")

# keep rows with at least N non-null values
df_clean = df.dropna(thresh=5)

# drop columns with any null
df_clean = df.dropna(axis=1)
```

### Filling Missing Data

```python
# fill with constant
df["phone"] = df["phone"].fillna("Unknown")
df["amount"] = df["amount"].fillna(0)

# fill with statistics
df["salary"] = df["salary"].fillna(df["salary"].mean())
df["salary"] = df["salary"].fillna(df["salary"].median())
df["department"] = df["department"].fillna(df["department"].mode()[0])

# fill with group-specific values
df["salary"] = df.groupby("department")["salary"].transform(lambda x: x.fillna(x.median()))

# forward fill / back fill
df["value"] = df["value"].ffill()      # carry previous value forward
df["value"] = df["value"].bfill()      # carry next value backward
df["value"] = df["value"].ffill(limit=3)  # fill at most 3 consecutive NaNs

# interpolate
df["temperature"] = df["temperature"].interpolate()                     # linear
df["temperature"] = df["temperature"].interpolate(method="time")        # time-based
df["temperature"] = df["temperature"].interpolate(method="polynomial", order=2)

# replace specific values with NaN
df = df.replace(["", "N/A", "null", "-", 0], np.nan)
df = df.replace({-999: np.nan, -1: np.nan})

# chain: replace then fill
df["status"] = df["status"].replace("", np.nan).fillna("unknown")
```

---

## 18. String Operations

All string methods are accessed via `.str` accessor on a Series.

```python
# --- case conversion ---
df["name"] = df["name"].str.lower()
df["name"] = df["name"].str.upper()
df["name"] = df["name"].str.title()          # "alice smith" → "Alice Smith"
df["name"] = df["name"].str.capitalize()     # "alice" → "Alice"

# --- whitespace ---
df["name"] = df["name"].str.strip()          # both sides
df["name"] = df["name"].str.lstrip()         # left
df["name"] = df["name"].str.rstrip()         # right
df["col"] = df["col"].str.replace(r"\s+", " ", regex=True)  # collapse whitespace

# --- contains / matching ---
mask = df["name"].str.contains("smith", case=False, na=False)
mask = df["name"].str.startswith("A")
mask = df["name"].str.endswith("son")
mask = df["name"].str.match(r"^[A-Z][a-z]+$")

# --- extract ---
df["area_code"] = df["phone"].str.extract(r"(\d{3})-\d{3}-\d{4}")
df[["first", "last"]] = df["name"].str.extract(r"(\w+)\s+(\w+)")

# --- extractall (multiple matches) ---
df["all_numbers"] = df["text"].str.findall(r"\d+")

# --- split ---
df[["first", "last"]] = df["name"].str.split(" ", n=1, expand=True)
df["domain"] = df["email"].str.split("@").str[1]

# --- replace ---
df["phone"] = df["phone"].str.replace("-", "", regex=False)
df["text"] = df["text"].str.replace(r"\d+", "NUM", regex=True)

# --- slice ---
df["zip"] = df["zipcode"].str[:5]
df["initial"] = df["name"].str[0]

# --- pad / justify ---
df["id"] = df["id"].str.zfill(8)              # "42" → "00000042"
df["code"] = df["code"].str.pad(10, side="right", fillchar="0")

# --- length ---
df["name_length"] = df["name"].str.len()

# --- concatenation ---
df["full"] = df["first"] + " " + df["last"]
df["full"] = df[["city", "state", "zip"]].astype(str).agg(", ".join, axis=1)

# --- get dummies from delimited string ---
df["tags"].str.get_dummies(sep=",")
```

---

## 19. Date and Time Operations in Pandas

```python
# --- convert to datetime ---
df["date"] = pd.to_datetime(df["date"])
df["date"] = pd.to_datetime(df["date"], format="%Y-%m-%d")
df["date"] = pd.to_datetime(df["date"], format="mixed", dayfirst=True)

# --- .dt accessor ---
df["year"]       = df["date"].dt.year
df["month"]      = df["date"].dt.month
df["day"]        = df["date"].dt.day
df["hour"]       = df["date"].dt.hour
df["weekday"]    = df["date"].dt.dayofweek      # 0=Mon, 6=Sun
df["day_name"]   = df["date"].dt.day_name()      # "Monday"
df["month_name"] = df["date"].dt.month_name()    # "January"
df["quarter"]    = df["date"].dt.quarter
df["week"]       = df["date"].dt.isocalendar().week
df["dayofyear"]  = df["date"].dt.dayofyear
df["is_weekend"] = df["date"].dt.dayofweek >= 5
df["is_month_end"] = df["date"].dt.is_month_end
df["is_month_start"] = df["date"].dt.is_month_start

# --- formatting ---
df["date_str"] = df["date"].dt.strftime("%Y-%m-%d")
df["month_year"] = df["date"].dt.strftime("%B %Y")     # "March 2024"

# --- arithmetic ---
df["days_since_hire"] = (pd.Timestamp.now() - df["hire_date"]).dt.days
df["next_review"] = df["hire_date"] + pd.DateOffset(years=1)
df["deadline"] = df["date"] + pd.Timedelta(days=30)

# difference between two dates
df["tenure_days"] = (df["end_date"] - df["start_date"]).dt.days
df["tenure_months"] = (df["end_date"] - df["start_date"]) / pd.Timedelta(days=30)

# --- floor / ceil / round ---
df["month_start"] = df["date"].dt.to_period("M").dt.to_timestamp()
df["date_floored"] = df["date"].dt.floor("D")          # remove time component
df["hour_floored"] = df["date"].dt.floor("h")          # round down to hour

# --- periods ---
df["month_period"] = df["date"].dt.to_period("M")      # 2024-03
df["quarter_period"] = df["date"].dt.to_period("Q")    # 2024Q1

# --- date ranges ---
dates = pd.date_range(start="2024-01-01", end="2024-12-31", freq="D")
dates = pd.date_range(start="2024-01-01", periods=12, freq="MS")     # month start
dates = pd.bdate_range(start="2024-01-01", end="2024-01-31")         # business days
```

### Frequency Aliases

| Alias | Meaning |
|-------|---------|
| `D` | Calendar day |
| `B` | Business day |
| `W` | Weekly |
| `MS` | Month start |
| `ME` | Month end |
| `QS` | Quarter start |
| `YS` | Year start |
| `h` | Hour |
| `min` | Minute |
| `s` | Second |

---

## 20. Sorting and Ranking

```python
# --- sort by values ---
df = df.sort_values("salary")                           # ascending (default)
df = df.sort_values("salary", ascending=False)          # descending
df = df.sort_values(["department", "salary"], ascending=[True, False])

# put NaN first or last
df = df.sort_values("salary", na_position="first")

# --- sort by index ---
df = df.sort_index()

# --- ranking ---
df["salary_rank"] = df["salary"].rank(ascending=False)
df["salary_rank"] = df["salary"].rank(method="dense")   # no gaps in rank

# rank within groups
df["dept_rank"] = df.groupby("department")["salary"].rank(ascending=False)

# ranking methods
# "average" (default): tied values get average rank
# "min": tied values get lowest rank (1, 1, 3)
# "max": tied values get highest rank (2, 2, 3)
# "dense": like min but no gaps (1, 1, 2)
# "first": tied values ranked by position

# --- nlargest / nsmallest ---
df.nlargest(5, "salary")             # top 5 by salary
df.nsmallest(3, "salary")            # bottom 3 by salary
```

---

## 21. Grouping and Aggregation

### Basic GroupBy

```python
# single aggregation
df.groupby("department")["salary"].mean()
df.groupby("department")["salary"].sum()
df.groupby("department").size()                  # count rows per group

# multiple aggregations
df.groupby("department")["salary"].agg(["mean", "median", "min", "max", "count"])

# named aggregation (clean output)
result = df.groupby("department").agg(
    avg_salary=("salary", "mean"),
    max_salary=("salary", "max"),
    headcount=("name", "count"),
    earliest_hire=("hire_date", "min"),
)

# multiple columns
df.groupby("department").agg({
    "salary": ["mean", "sum"],
    "name": "count",
    "hire_date": "max",
})

# group by multiple columns
df.groupby(["department", "level"]).agg(
    avg_salary=("salary", "mean"),
    count=("id", "count"),
)
```

### Custom Aggregation Functions

```python
# with lambda
df.groupby("department")["salary"].agg(lambda x: x.max() - x.min())

# with named function
def coefficient_of_variation(x):
    return x.std() / x.mean()

df.groupby("department")["salary"].agg(coefficient_of_variation)

# multiple custom + built-in
df.groupby("department")["salary"].agg(
    mean="mean",
    range=lambda x: x.max() - x.min(),
    iqr=lambda x: x.quantile(0.75) - x.quantile(0.25),
)
```

### GroupBy + Transform

Returns same-shaped output (broadcasts back to original DataFrame).

```python
# add group mean as a new column
df["dept_avg"] = df.groupby("department")["salary"].transform("mean")

# percent of group total
df["pct_of_dept"] = df["salary"] / df.groupby("department")["salary"].transform("sum")

# z-score within group
df["salary_zscore"] = df.groupby("department")["salary"].transform(
    lambda x: (x - x.mean()) / x.std()
)

# fill nulls with group median
df["salary"] = df.groupby("department")["salary"].transform(lambda x: x.fillna(x.median()))

# flag groups
df["is_large_dept"] = df.groupby("department")["id"].transform("count") > 10
```

### GroupBy + Filter

Keep/drop entire groups based on a condition.

```python
# keep only departments with more than 5 employees
df_filtered = df.groupby("department").filter(lambda g: len(g) > 5)

# keep groups where average salary > 70k
df_filtered = df.groupby("department").filter(lambda g: g["salary"].mean() > 70000)
```

### GroupBy Iteration

```python
for dept, group_df in df.groupby("department"):
    print(f"{dept}: {len(group_df)} employees")
    process(group_df)

# get a specific group
eng_df = df.groupby("department").get_group("Engineering")
```

---

## 22. Merging, Joining, and Concatenating

### merge (SQL-style Joins)

```python
# inner join (default)
merged = pd.merge(orders, customers, on="customer_id")

# left join
merged = pd.merge(orders, customers, on="customer_id", how="left")

# right join
merged = pd.merge(orders, customers, on="customer_id", how="right")

# outer join
merged = pd.merge(orders, customers, on="customer_id", how="outer")

# join on different column names
merged = pd.merge(orders, customers, left_on="cust_id", right_on="customer_id")

# join on multiple columns
merged = pd.merge(df1, df2, on=["year", "month", "product"])

# handle duplicate column names
merged = pd.merge(df1, df2, on="id", suffixes=("_left", "_right"))

# indicator column (shows where rows came from)
merged = pd.merge(df1, df2, on="id", how="outer", indicator=True)
# _merge column: "left_only", "right_only", "both"

# validate join cardinality
merged = pd.merge(df1, df2, on="id", validate="one_to_one")   # raises if not 1:1
# "one_to_one", "one_to_many", "many_to_one", "many_to_many"

# cross join (Cartesian product)
cross = pd.merge(df1, df2, how="cross")
```

### concat (Stack DataFrames)

```python
# stack vertically (union)
combined = pd.concat([df1, df2, df3], ignore_index=True)

# stack horizontally (column-bind)
combined = pd.concat([df1, df2], axis=1)

# combine CSVs from a folder
from pathlib import Path
csv_files = Path("data/").glob("*.csv")
df = pd.concat([pd.read_csv(f) for f in csv_files], ignore_index=True)

# with keys (track source)
combined = pd.concat([df1, df2], keys=["file1", "file2"], names=["source"])
```

### join (Index-based)

```python
# join on index
df1.join(df2, how="left")

# join on index with different index names
df1.join(df2.set_index("key"), on="key", how="left")
```

### Anti-Join (Rows in A not in B)

```python
# find customers who haven't ordered
no_orders = customers[~customers["customer_id"].isin(orders["customer_id"])]

# using merge + indicator
merged = pd.merge(customers, orders[["customer_id"]], on="customer_id", how="left", indicator=True)
no_orders = merged[merged["_merge"] == "left_only"].drop(columns="_merge")
```

---

## 23. Pivot Tables and Reshaping

### Pivot Table

```python
# basic pivot
pt = df.pivot_table(
    values="salary",
    index="department",
    aggfunc="mean"
)

# with columns (cross-tab)
pt = df.pivot_table(
    values="salary",
    index="department",
    columns="level",
    aggfunc="mean",
    fill_value=0,
)

# multiple aggregations
pt = df.pivot_table(
    values="salary",
    index="department",
    aggfunc=["mean", "count", "sum"],
    margins=True,           # add totals row/column
    margins_name="Total",
)

# multiple values
pt = df.pivot_table(
    values=["salary", "bonus"],
    index="department",
    columns="year",
    aggfunc="sum",
)
```

### Pivot (Reshape without Aggregation)

```python
# long to wide (no duplicates in index+columns)
wide = df.pivot(index="date", columns="product", values="revenue")
```

### Melt (Wide to Long)

```python
# convert columns to rows
long = pd.melt(
    df,
    id_vars=["name", "department"],      # keep these columns
    value_vars=["q1", "q2", "q3", "q4"], # unpivot these
    var_name="quarter",                   # name for the variable column
    value_name="revenue",                 # name for the value column
)
```

### Crosstab

```python
# frequency table
ct = pd.crosstab(df["department"], df["level"])

# with normalized values
ct = pd.crosstab(df["department"], df["level"], normalize="index")  # row percentages

# with custom aggfunc
ct = pd.crosstab(df["department"], df["level"], values=df["salary"], aggfunc="mean")
```

### Stack and Unstack

```python
# stack: columns → rows (wide to long)
stacked = df.set_index(["department", "year"]).stack()

# unstack: rows → columns (long to wide)
unstacked = stacked.unstack(level="year")
```

### Explode (Expand Lists into Rows)

```python
# if a column contains lists
df = pd.DataFrame({
    "name": ["Alice", "Bob"],
    "skills": [["Python", "SQL"], ["Java", "Go", "Rust"]]
})
df.explode("skills")
# Alice  Python
# Alice  SQL
# Bob    Java
# Bob    Go
# Bob    Rust
```

---

## 24. Apply, Map, and Transform

### apply

```python
# apply function to each element of a Series
df["name_clean"] = df["name"].apply(str.title)
df["salary_k"] = df["salary"].apply(lambda x: f"${x/1000:.0f}k")

# apply function to each row
df["full_info"] = df.apply(lambda row: f"{row['name']} ({row['department']})", axis=1)

# apply to entire DataFrame column-wise
df[["col1", "col2"]].apply(np.sum)        # sum of each column

# apply with additional arguments
def categorize(value, thresholds, labels):
    for threshold, label in zip(thresholds, labels):
        if value < threshold:
            return label
    return labels[-1]

df["tier"] = df["salary"].apply(categorize, thresholds=[50000, 80000], labels=["Low", "Mid", "High"])
```

### map

```python
# map values using a dictionary
status_map = {0: "inactive", 1: "active", 2: "pending"}
df["status_label"] = df["status_code"].map(status_map)

# map with function
df["salary_rounded"] = df["salary"].map(lambda x: round(x, -3))

# map with a Series (useful for lookups)
dept_manager = df.groupby("department")["manager"].first()
df["dept_manager"] = df["department"].map(dept_manager)
```

### replace

```python
# replace specific values
df["status"] = df["status"].replace({"A": "Active", "I": "Inactive"})
df["col"] = df["col"].replace([999, -1], np.nan)

# regex replace
df["text"] = df["text"].replace(r"\d+", "NUM", regex=True)
```

### transform vs apply in GroupBy

```python
# transform: returns same shape, broadcasts group result
df["dept_mean"] = df.groupby("dept")["salary"].transform("mean")

# apply: returns aggregated result (one row per group)
result = df.groupby("dept")["salary"].apply(list)
```

### applymap / map on DataFrame

```python
# apply function to every cell (renamed to .map in pandas 2.1+)
df_formatted = df.select_dtypes(include="number").map(lambda x: f"{x:,.2f}")

# older pandas: applymap
df_formatted = df.select_dtypes(include="number").applymap(lambda x: f"{x:,.2f}")
```

### pipe (Method Chaining)

```python
def remove_outliers(df, col, n_std=3):
    mean, std = df[col].mean(), df[col].std()
    return df[(df[col] - mean).abs() <= n_std * std]

def add_features(df):
    return df.assign(
        tenure_years=(pd.Timestamp.now() - df["hire_date"]).dt.days / 365.25,
        salary_rank=df["salary"].rank(ascending=False),
    )

result = (
    df
    .pipe(remove_outliers, "salary")
    .pipe(add_features)
    .query("tenure_years > 1")
    .sort_values("salary_rank")
)
```

---

## 25. Window Functions (Rolling, Expanding, Shifting)

### Rolling (Moving Window)

```python
# 7-day moving average
df["ma_7"] = df["revenue"].rolling(window=7).mean()

# rolling with min_periods (allow partial windows)
df["ma_7"] = df["revenue"].rolling(window=7, min_periods=1).mean()

# other rolling aggregations
df["rolling_sum"] = df["revenue"].rolling(7).sum()
df["rolling_max"] = df["revenue"].rolling(7).max()
df["rolling_std"] = df["revenue"].rolling(7).std()

# centered window
df["ma_centered"] = df["revenue"].rolling(window=7, center=True).mean()

# time-based window (requires DatetimeIndex)
df = df.set_index("date")
df["ma_30d"] = df["revenue"].rolling("30D").mean()    # 30 calendar days

# rolling with custom function
df["rolling_range"] = df["price"].rolling(5).apply(lambda x: x.max() - x.min())

# rolling correlation
df["corr_rolling"] = df["price"].rolling(20).corr(df["volume"])
```

### Expanding (Cumulative)

```python
# cumulative mean (all rows up to current)
df["cum_mean"] = df["revenue"].expanding().mean()

# cumulative sum
df["cum_sum"] = df["revenue"].expanding().sum()
df["cum_sum"] = df["revenue"].cumsum()          # shortcut

# cumulative min/max
df["cum_max"] = df["revenue"].cummax()
df["cum_min"] = df["revenue"].cummin()

# cumulative product
df["cum_product"] = (1 + df["return_pct"]).cumprod()

# cumulative count
df["running_count"] = df.groupby("department").cumcount() + 1
```

### Shifting (Lag / Lead)

```python
# lag: previous values
df["prev_revenue"] = df["revenue"].shift(1)          # 1 period back
df["prev_3"] = df["revenue"].shift(3)                 # 3 periods back

# lead: future values
df["next_revenue"] = df["revenue"].shift(-1)          # 1 period forward

# period-over-period change
df["change"] = df["revenue"] - df["revenue"].shift(1)
df["pct_change"] = df["revenue"].pct_change()         # percentage change

# year-over-year (shift by 12 months)
df["yoy_change"] = df["revenue"].pct_change(periods=12)

# grouped shift
df["prev_in_dept"] = df.groupby("department")["salary"].shift(1)
```

### EWM (Exponentially Weighted)

```python
# exponential moving average
df["ema_12"] = df["price"].ewm(span=12).mean()
df["ema_26"] = df["price"].ewm(span=26).mean()

# MACD (technical indicator example)
df["macd"] = df["ema_12"] - df["ema_26"]
```

---

## 26. Duplicates and Deduplication

```python
# --- detect duplicates ---
df.duplicated()                               # boolean Series, True for duplicate rows
df.duplicated(subset=["name", "email"])       # check specific columns
df.duplicated(keep="first")                   # mark all but first occurrence
df.duplicated(keep="last")                    # mark all but last occurrence
df.duplicated(keep=False)                     # mark ALL duplicates

# count duplicates
df.duplicated().sum()

# view duplicate rows
df[df.duplicated(subset=["email"], keep=False)].sort_values("email")

# --- remove duplicates ---
df = df.drop_duplicates()                                     # all columns
df = df.drop_duplicates(subset=["email"])                     # by specific columns
df = df.drop_duplicates(subset=["email"], keep="last")        # keep last occurrence
df = df.drop_duplicates(subset=["email"], keep=False)         # drop ALL duplicates

# deduplicate keeping the row with highest salary
df = df.sort_values("salary", ascending=False).drop_duplicates(subset=["employee_id"], keep="first")

# deduplicate keeping most recent
df = df.sort_values("updated_at", ascending=False).drop_duplicates(subset=["id"], keep="first")
```

---

## 27. Binning, Cutting, and Categorization

### pd.cut (Equal-Width Bins)

```python
# fixed bins
df["salary_bin"] = pd.cut(df["salary"], bins=5)                           # 5 equal-width bins
df["salary_bin"] = pd.cut(df["salary"], bins=[0, 50000, 80000, 120000, float("inf")])

# with labels
df["salary_tier"] = pd.cut(
    df["salary"],
    bins=[0, 50000, 80000, 120000, float("inf")],
    labels=["Entry", "Mid", "Senior", "Executive"],
)
```

### pd.qcut (Equal-Frequency Bins)

```python
# quartiles (each bin has ~25% of data)
df["salary_quartile"] = pd.qcut(df["salary"], q=4, labels=["Q1", "Q2", "Q3", "Q4"])

# deciles
df["salary_decile"] = pd.qcut(df["salary"], q=10, labels=False)   # 0-9

# custom quantiles
df["tier"] = pd.qcut(df["score"], q=[0, 0.25, 0.75, 1.0], labels=["Low", "Mid", "High"])
```

### np.select (Multiple Conditions)

```python
conditions = [
    (df["age"] < 18),
    (df["age"] < 35),
    (df["age"] < 55),
    (df["age"] >= 55),
]
labels = ["Under 18", "Young Adult", "Middle Aged", "Senior"]
df["age_group"] = np.select(conditions, labels, default="Unknown")
```

---

## 28. MultiIndex and Advanced Indexing

```python
# create MultiIndex
df = df.set_index(["department", "level"])

# access with MultiIndex
df.loc["Engineering"]                          # all Engineering rows
df.loc[("Engineering", "Senior")]              # specific combination
df.loc[("Engineering", "Senior"), "salary"]    # specific value

# reset index
df = df.reset_index()

# swap levels
df = df.swaplevel(0, 1)

# sort MultiIndex (required for slicing)
df = df.sort_index()

# cross-section
df.xs("Senior", level="level")       # all departments, Senior level

# MultiIndex columns (from pivot tables)
pt = df.pivot_table(values="salary", index="department", columns="level", aggfunc=["mean", "count"])
pt.columns                            # MultiIndex: [('mean', 'Junior'), ('mean', 'Senior'), ...]
pt[("mean", "Senior")]               # select specific column
pt.columns = ["_".join(col) for col in pt.columns]   # flatten
```

---

# Part 4: Data Cleaning Patterns

---

## 29. End-to-End Cleaning Pipeline

```python
import pandas as pd
import numpy as np

def clean_dataframe(df):
    """Standard cleaning pipeline applicable to most datasets."""
    df = df.copy()

    # 1. clean column names
    df.columns = (
        df.columns
        .str.lower()
        .str.replace(r"[^\w]", "_", regex=True)
        .str.replace(r"_+", "_", regex=True)
        .str.strip("_")
    )

    # 2. drop fully empty rows and columns
    df = df.dropna(how="all")
    df = df.dropna(axis=1, how="all")

    # 3. drop unnamed/index columns
    df = df.loc[:, ~df.columns.str.contains("^unnamed", case=False)]

    # 4. strip whitespace from string columns
    str_cols = df.select_dtypes(include="object").columns
    df[str_cols] = df[str_cols].apply(lambda x: x.str.strip())

    # 5. replace blank strings with NaN
    df = df.replace(r"^\s*$", np.nan, regex=True)

    # 6. drop exact duplicate rows
    df = df.drop_duplicates()

    # 7. reset index
    df = df.reset_index(drop=True)

    return df

# usage
df = pd.read_csv("raw_data.csv")
df = clean_dataframe(df)
```

### Column-Specific Cleaning

```python
def clean_phone(series):
    return (
        series.astype(str)
        .str.replace(r"[^\d]", "", regex=True)    # remove non-digits
        .str[-10:]                                  # keep last 10 digits
        .replace("", np.nan)
    )

def clean_email(series):
    return (
        series.str.lower()
        .str.strip()
        .where(series.str.contains(r"^[\w.+-]+@[\w-]+\.[\w.]+$", na=False))
    )

def clean_currency(series):
    return (
        series.astype(str)
        .str.replace(r"[$,]", "", regex=True)
        .pipe(pd.to_numeric, errors="coerce")
    )

df["phone"] = clean_phone(df["phone"])
df["email"] = clean_email(df["email"])
df["revenue"] = clean_currency(df["revenue"])
```

---

## 30. Outlier Detection and Treatment

### Z-Score Method

```python
from scipy import stats

# flag outliers (|z| > 3)
df["z_score"] = stats.zscore(df["salary"])
outliers = df[df["z_score"].abs() > 3]

# remove outliers
df_clean = df[df["z_score"].abs() <= 3]
```

### IQR Method

```python
Q1 = df["salary"].quantile(0.25)
Q3 = df["salary"].quantile(0.75)
IQR = Q3 - Q1
lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR

outliers = df[(df["salary"] < lower) | (df["salary"] > upper)]
df_clean = df[(df["salary"] >= lower) & (df["salary"] <= upper)]

# cap instead of remove (winsorize)
df["salary_capped"] = df["salary"].clip(lower=lower, upper=upper)
```

### Percentile Capping

```python
lower = df["value"].quantile(0.01)
upper = df["value"].quantile(0.99)
df["value_capped"] = df["value"].clip(lower, upper)
```

---

## 31. Data Validation and Quality Checks

```python
def validate_dataframe(df, rules):
    """Run validation checks and report issues."""
    issues = []

    for col, checks in rules.items():
        if col not in df.columns:
            issues.append(f"Missing column: {col}")
            continue

        if "not_null" in checks:
            null_count = df[col].isnull().sum()
            if null_count > 0:
                issues.append(f"{col}: {null_count} nulls ({null_count/len(df):.1%})")

        if "unique" in checks:
            dups = df[col].duplicated().sum()
            if dups > 0:
                issues.append(f"{col}: {dups} duplicate values")

        if "min" in checks:
            violations = (df[col] < checks["min"]).sum()
            if violations:
                issues.append(f"{col}: {violations} values below {checks['min']}")

        if "max" in checks:
            violations = (df[col] > checks["max"]).sum()
            if violations:
                issues.append(f"{col}: {violations} values above {checks['max']}")

        if "values" in checks:
            invalid = ~df[col].isin(checks["values"])
            if invalid.sum():
                issues.append(f"{col}: {invalid.sum()} invalid values")

        if "regex" in checks:
            invalid = ~df[col].astype(str).str.match(checks["regex"], na=False)
            non_null_invalid = invalid & df[col].notna()
            if non_null_invalid.sum():
                issues.append(f"{col}: {non_null_invalid.sum()} don't match pattern")

    return issues

# usage
rules = {
    "email": {"not_null": True, "unique": True, "regex": r"^[\w.+-]+@[\w-]+\.\w+$"},
    "age": {"not_null": True, "min": 0, "max": 150},
    "status": {"values": ["active", "inactive", "pending"]},
    "salary": {"not_null": True, "min": 0},
}

issues = validate_dataframe(df, rules)
for issue in issues:
    print(f"  ⚠ {issue}")
```

### Quick Quality Report

```python
def quality_report(df):
    report = pd.DataFrame({
        "dtype": df.dtypes,
        "non_null": df.notna().sum(),
        "null_count": df.isnull().sum(),
        "null_pct": (df.isnull().mean() * 100).round(1),
        "unique": df.nunique(),
        "sample": df.iloc[0] if len(df) > 0 else None,
    })
    return report.sort_values("null_pct", ascending=False)

print(quality_report(df).to_string())
```

---

# Part 5: NumPy Essentials

---

## 32. NumPy Arrays and Operations

```python
import numpy as np

# --- creating arrays ---
a = np.array([1, 2, 3, 4, 5])
b = np.array([[1, 2, 3], [4, 5, 6]])            # 2D
zeros = np.zeros((3, 4))                          # 3x4 of zeros
ones = np.ones((2, 3))                            # 2x3 of ones
empty = np.empty((3, 3))                          # uninitialized (fast)
identity = np.eye(4)                              # 4x4 identity matrix
rng = np.arange(0, 10, 2)                         # [0, 2, 4, 6, 8]
lin = np.linspace(0, 1, 5)                        # [0, 0.25, 0.5, 0.75, 1.0]
full = np.full((3, 3), fill_value=7)              # 3x3 of sevens

# --- random ---
np.random.seed(42)                                # reproducibility
np.random.rand(3, 4)                              # uniform [0, 1)
np.random.randn(3, 4)                             # standard normal
np.random.randint(1, 100, size=(5,))              # random ints
np.random.choice(["a", "b", "c"], size=10)        # sample with replacement
np.random.shuffle(arr)                            # in-place shuffle

# new-style random (recommended)
rng = np.random.default_rng(42)
rng.random((3, 4))
rng.normal(loc=0, scale=1, size=(3, 4))
rng.integers(1, 100, size=5)

# --- properties ---
a.shape         # (5,)
a.ndim          # 1
a.dtype         # dtype('int64')
a.size          # 5 (total elements)

# --- reshaping ---
a = np.arange(12)
a.reshape(3, 4)         # 3 rows x 4 cols
a.reshape(-1, 3)        # auto-compute rows, 3 cols
a.flatten()             # to 1D (copy)
a.ravel()               # to 1D (view, no copy)
a.T                     # transpose

# --- indexing and slicing ---
a = np.array([10, 20, 30, 40, 50])
a[0]                 # 10
a[-1]                # 50
a[1:4]               # [20, 30, 40]
a[::2]               # [10, 30, 50]

# boolean indexing
a[a > 25]            # [30, 40, 50]

# fancy indexing
a[[0, 2, 4]]         # [10, 30, 50]

# 2D
b = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
b[0, 1]              # 2
b[:, 1]              # column 1: [2, 5, 8]
b[1, :]              # row 1: [4, 5, 6]

# --- element-wise operations ---
a + b
a * b
a ** 2
np.sqrt(a)
np.log(a)
np.exp(a)

# --- aggregations ---
a.sum()
a.mean()
a.std()
a.min(), a.max()
a.argmin(), a.argmax()      # index of min/max
a.cumsum()                   # cumulative sum
a.cumprod()                  # cumulative product

# along an axis
b.sum(axis=0)               # sum each column
b.sum(axis=1)               # sum each row
b.mean(axis=0)

# --- conditions ---
np.where(a > 30, "high", "low")                   # vectorized if-else
np.select([a < 20, a < 40], ["low", "mid"], "high")
np.clip(a, 15, 45)                                 # cap values

# --- set operations ---
np.unique(a)                     # sorted unique values
np.intersect1d(a, b)             # elements in both
np.union1d(a, b)                 # elements in either
np.setdiff1d(a, b)              # in a but not in b
np.isin(a, [10, 30, 50])        # boolean mask
```

---

## 33. Linear Algebra and Statistics with NumPy

```python
# --- linear algebra ---
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

A @ B                             # matrix multiplication
np.dot(A, B)                      # same thing
np.linalg.inv(A)                  # inverse
np.linalg.det(A)                  # determinant
np.linalg.eig(A)                  # eigenvalues and eigenvectors
np.linalg.norm(A)                 # Frobenius norm
np.linalg.solve(A, b)            # solve Ax = b

# --- statistics ---
data = np.random.randn(1000)
np.mean(data)
np.median(data)
np.std(data)
np.var(data)
np.percentile(data, [25, 50, 75])
np.corrcoef(x, y)                # correlation matrix
np.cov(x, y)                     # covariance matrix

# weighted average
np.average(values, weights=weights)

# histogram
counts, bin_edges = np.histogram(data, bins=20)
```

---

# Part 6: Data Visualization

---

## 34. Matplotlib Fundamentals

```python
import matplotlib.pyplot as plt
import matplotlib.dates as mdates

# --- basic plots ---

# line plot
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(df["date"], df["revenue"], label="Revenue", color="steelblue", linewidth=2)
ax.set_title("Monthly Revenue")
ax.set_xlabel("Date")
ax.set_ylabel("Revenue ($)")
ax.legend()
plt.tight_layout()
plt.savefig("revenue.png", dpi=150, bbox_inches="tight")
plt.show()

# bar chart
fig, ax = plt.subplots(figsize=(10, 6))
ax.bar(df["department"], df["headcount"], color="steelblue")
ax.set_title("Headcount by Department")
ax.set_ylabel("Employees")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# horizontal bar
ax.barh(df["department"], df["salary"])

# scatter plot
ax.scatter(df["experience"], df["salary"], alpha=0.6, c=df["department_code"], cmap="viridis")

# histogram
ax.hist(df["salary"], bins=30, edgecolor="white", alpha=0.7)

# box plot
ax.boxplot([dept1_salaries, dept2_salaries], labels=["Engineering", "Sales"])

# pie chart
ax.pie(sizes, labels=labels, autopct="%1.1f%%", startangle=90)

# --- subplots ---
fig, axes = plt.subplots(2, 2, figsize=(14, 10))
axes[0, 0].plot(x, y1)
axes[0, 0].set_title("Plot 1")
axes[0, 1].bar(categories, values)
axes[0, 1].set_title("Plot 2")
axes[1, 0].scatter(x, y2)
axes[1, 1].hist(data, bins=20)
plt.tight_layout()
plt.show()

# --- formatting ---
ax.set_xlim(0, 100)
ax.set_ylim(0, 200000)
ax.grid(True, alpha=0.3)
ax.axhline(y=mean_val, color="red", linestyle="--", label="Mean")
ax.axvline(x=threshold, color="gray", linestyle=":")
ax.annotate("Peak", xy=(peak_x, peak_y), fontsize=12)
ax.set_xticks(range(0, 13))
ax.xaxis.set_major_formatter(mdates.DateFormatter("%b %Y"))
plt.xticks(rotation=45)

# --- pandas built-in plotting ---
df["salary"].plot(kind="hist", bins=30)
df.plot(x="date", y="revenue", kind="line")
df.groupby("department")["salary"].mean().plot(kind="bar")
df.plot.scatter(x="experience", y="salary")
df[["q1", "q2", "q3", "q4"]].plot(kind="box")
```

---

## 35. Seaborn for Statistical Plots

```python
import seaborn as sns

sns.set_theme(style="whitegrid")

# distribution plot
sns.histplot(df["salary"], bins=30, kde=True)

# KDE (kernel density)
sns.kdeplot(data=df, x="salary", hue="department")

# box plot (with groups)
sns.boxplot(data=df, x="department", y="salary")

# violin plot
sns.violinplot(data=df, x="department", y="salary")

# strip/swarm (individual points)
sns.stripplot(data=df, x="department", y="salary", jitter=True, alpha=0.5)
sns.swarmplot(data=df, x="department", y="salary")

# scatter with regression
sns.lmplot(data=df, x="experience", y="salary", hue="department")
sns.regplot(data=df, x="experience", y="salary")

# pair plot (all variable combinations)
sns.pairplot(df[["salary", "experience", "age", "department"]], hue="department")

# correlation heatmap
corr = df[["salary", "experience", "age", "performance"]].corr()
sns.heatmap(corr, annot=True, cmap="coolwarm", center=0, fmt=".2f")

# count plot
sns.countplot(data=df, x="department", order=df["department"].value_counts().index)

# bar plot (with confidence intervals)
sns.barplot(data=df, x="department", y="salary", estimator=np.mean)

# line plot (with confidence bands)
sns.lineplot(data=df, x="month", y="revenue", hue="product")

# facet grid
g = sns.FacetGrid(df, col="department", col_wrap=3, height=4)
g.map(sns.histplot, "salary")

# categorical heatmap
pivot = df.pivot_table(values="salary", index="department", columns="level", aggfunc="mean")
sns.heatmap(pivot, annot=True, fmt=",.0f", cmap="YlOrRd")
```

---

## 36. Plotly for Interactive Visualizations

```python
import plotly.express as px
import plotly.graph_objects as go

# line
fig = px.line(df, x="date", y="revenue", color="product", title="Revenue Over Time")
fig.show()

# bar
fig = px.bar(df_agg, x="department", y="headcount", color="level", barmode="group")

# scatter
fig = px.scatter(df, x="experience", y="salary", color="department", size="performance",
                 hover_data=["name"], trendline="ols")

# histogram
fig = px.histogram(df, x="salary", nbins=30, color="department", marginal="box")

# box
fig = px.box(df, x="department", y="salary", color="level", points="outliers")

# heatmap
fig = px.imshow(corr_matrix, text_auto=".2f", color_continuous_scale="RdBu_r")

# sunburst / treemap
fig = px.sunburst(df, path=["region", "country", "city"], values="revenue")
fig = px.treemap(df, path=["department", "team", "name"], values="salary")

# geographic
fig = px.choropleth(df, locations="country_code", color="revenue",
                    locationmode="ISO-3", title="Revenue by Country")

# subplots with plotly
from plotly.subplots import make_subplots
fig = make_subplots(rows=2, cols=2, subplot_titles=["A", "B", "C", "D"])
fig.add_trace(go.Bar(x=x, y=y), row=1, col=1)
fig.add_trace(go.Scatter(x=x, y=y2), row=1, col=2)
fig.update_layout(height=800, width=1200, title_text="Dashboard")
fig.show()

# save
fig.write_html("dashboard.html")
fig.write_image("chart.png")
```

---

# Part 7: Feature Engineering and ML Prep

---

## 37. Feature Engineering Patterns

```python
# --- date features ---
df["year"] = df["date"].dt.year
df["month"] = df["date"].dt.month
df["day_of_week"] = df["date"].dt.dayofweek
df["is_weekend"] = df["date"].dt.dayofweek >= 5
df["quarter"] = df["date"].dt.quarter
df["is_month_start"] = df["date"].dt.is_month_start
df["days_since_event"] = (pd.Timestamp.now() - df["event_date"]).dt.days

# --- text features ---
df["text_length"] = df["description"].str.len()
df["word_count"] = df["description"].str.split().str.len()
df["has_email"] = df["text"].str.contains(r"[\w.+-]+@[\w-]+\.\w+", na=False)

# --- aggregation features ---
df["dept_avg_salary"] = df.groupby("department")["salary"].transform("mean")
df["dept_salary_rank"] = df.groupby("department")["salary"].rank(ascending=False)
df["salary_vs_dept_avg"] = df["salary"] / df["dept_avg_salary"]

# --- interaction features ---
df["price_per_unit"] = df["total_price"] / df["quantity"]
df["bmi"] = df["weight"] / (df["height"] / 100) ** 2

# --- binning ---
df["age_group"] = pd.cut(df["age"], bins=[0, 18, 35, 55, 100], labels=["Youth", "Young", "Mid", "Senior"])

# --- lag features (time series) ---
df["revenue_lag1"] = df.groupby("product")["revenue"].shift(1)
df["revenue_lag7"] = df.groupby("product")["revenue"].shift(7)
df["revenue_ma7"] = df.groupby("product")["revenue"].transform(lambda x: x.rolling(7).mean())
df["revenue_change"] = df["revenue"].pct_change()

# --- ratio features ---
df["expense_ratio"] = df["expenses"] / df["revenue"]
df["profit_margin"] = (df["revenue"] - df["cost"]) / df["revenue"]

# --- log transform (reduce skew) ---
df["log_salary"] = np.log1p(df["salary"])      # log(1 + x), handles zeros
df["log_revenue"] = np.log(df["revenue"])
```

---

## 38. Encoding Categorical Variables

```python
from sklearn.preprocessing import LabelEncoder, OrdinalEncoder, OneHotEncoder

# --- one-hot encoding with Pandas ---
df_encoded = pd.get_dummies(df, columns=["department", "level"], drop_first=True, dtype=int)

# --- label encoding (for ordinal data) ---
le = LabelEncoder()
df["dept_encoded"] = le.fit_transform(df["department"])
le.inverse_transform([0, 1, 2])      # decode back

# --- ordinal encoding (explicit ordering) ---
oe = OrdinalEncoder(categories=[["Low", "Medium", "High"]])
df["priority_encoded"] = oe.fit_transform(df[["priority"]])

# --- frequency encoding ---
freq = df["city"].value_counts(normalize=True)
df["city_freq"] = df["city"].map(freq)

# --- target encoding (mean of target per category) ---
target_mean = df.groupby("city")["target"].mean()
df["city_target_enc"] = df["city"].map(target_mean)

# --- binary encoding (for high-cardinality) ---
import category_encoders as ce
encoder = ce.BinaryEncoder(cols=["zipcode"])
df_encoded = encoder.fit_transform(df)

# --- hash encoding (for very high cardinality) ---
encoder = ce.HashingEncoder(cols=["user_id"], n_components=8)
df_encoded = encoder.fit_transform(df)
```

---

## 39. Scaling and Normalization

```python
from sklearn.preprocessing import StandardScaler, MinMaxScaler, RobustScaler, MaxAbsScaler

# --- Standard Scaler (z-score: mean=0, std=1) ---
scaler = StandardScaler()
df[["salary_scaled", "age_scaled"]] = scaler.fit_transform(df[["salary", "age"]])

# --- MinMax Scaler (scale to [0, 1]) ---
scaler = MinMaxScaler()
df[["salary_norm"]] = scaler.fit_transform(df[["salary"]])

# --- Robust Scaler (uses median/IQR, resistant to outliers) ---
scaler = RobustScaler()
df[["salary_robust"]] = scaler.fit_transform(df[["salary"]])

# --- fit on train, transform both ---
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)         # use train's mean/std

# --- manual z-score ---
df["salary_z"] = (df["salary"] - df["salary"].mean()) / df["salary"].std()

# --- log transform ---
df["salary_log"] = np.log1p(df["salary"])

# --- power transform (reduce skew) ---
from sklearn.preprocessing import PowerTransformer
pt = PowerTransformer(method="yeo-johnson")
df[["salary_pt"]] = pt.fit_transform(df[["salary"]])
```

---

## 40. Train-Test Split and Cross Validation

```python
from sklearn.model_selection import (
    train_test_split, KFold, StratifiedKFold,
    cross_val_score, GridSearchCV, RandomizedSearchCV,
)

# --- basic split ---
X = df.drop(columns="target")
y = df["target"]

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# stratified split (preserves class distribution)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

# train/validation/test (80/10/10)
X_temp, X_test, y_temp, y_test = train_test_split(X, y, test_size=0.1, random_state=42)
X_train, X_val, y_train, y_val = train_test_split(X_temp, y_temp, test_size=0.111, random_state=42)

# --- cross validation ---
scores = cross_val_score(model, X, y, cv=5, scoring="accuracy")
print(f"Mean: {scores.mean():.3f} ± {scores.std():.3f}")

# --- K-Fold ---
kf = KFold(n_splits=5, shuffle=True, random_state=42)
for train_idx, val_idx in kf.split(X):
    X_train, X_val = X.iloc[train_idx], X.iloc[val_idx]
    y_train, y_val = y.iloc[train_idx], y.iloc[val_idx]
    model.fit(X_train, y_train)
    score = model.score(X_val, y_val)

# --- Stratified K-Fold (for imbalanced classes) ---
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
for train_idx, val_idx in skf.split(X, y):
    ...

# --- Grid Search ---
param_grid = {
    "n_estimators": [100, 200, 500],
    "max_depth": [5, 10, 20, None],
    "min_samples_split": [2, 5, 10],
}
grid = GridSearchCV(model, param_grid, cv=5, scoring="accuracy", n_jobs=-1)
grid.fit(X_train, y_train)
print(grid.best_params_)
print(grid.best_score_)
best_model = grid.best_estimator_

# --- Randomized Search (faster for large parameter spaces) ---
from scipy.stats import randint, uniform
param_dist = {
    "n_estimators": randint(100, 1000),
    "max_depth": randint(3, 30),
    "learning_rate": uniform(0.01, 0.3),
}
search = RandomizedSearchCV(model, param_dist, n_iter=50, cv=5, random_state=42, n_jobs=-1)
search.fit(X_train, y_train)
```

---

# Part 8: Machine Learning with Scikit-Learn

---

## 41. Regression Models

```python
from sklearn.linear_model import LinearRegression, Ridge, Lasso, ElasticNet
from sklearn.ensemble import RandomForestRegressor, GradientBoostingRegressor
from sklearn.svm import SVR
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score

# --- Linear Regression ---
model = LinearRegression()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

print(f"Coefficients: {dict(zip(X.columns, model.coef_))}")
print(f"Intercept: {model.intercept_:.2f}")
print(f"R²: {r2_score(y_test, y_pred):.3f}")
print(f"RMSE: {mean_squared_error(y_test, y_pred, squared=False):.2f}")
print(f"MAE: {mean_absolute_error(y_test, y_pred):.2f}")

# --- Ridge (L2 regularization) ---
model = Ridge(alpha=1.0)
model.fit(X_train, y_train)

# --- Lasso (L1 regularization, feature selection) ---
model = Lasso(alpha=0.1)
model.fit(X_train, y_train)
important_features = pd.Series(model.coef_, index=X.columns)
print(important_features[important_features != 0].sort_values())

# --- Random Forest Regressor ---
model = RandomForestRegressor(n_estimators=200, max_depth=10, random_state=42, n_jobs=-1)
model.fit(X_train, y_train)

# feature importance
importance = pd.Series(model.feature_importances_, index=X.columns).sort_values(ascending=False)
importance.head(10).plot(kind="barh")

# --- Gradient Boosting ---
model = GradientBoostingRegressor(n_estimators=200, learning_rate=0.1, max_depth=5, random_state=42)
model.fit(X_train, y_train)
```

---

## 42. Classification Models

```python
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.svm import SVC
from sklearn.neighbors import KNeighborsClassifier
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score, f1_score,
    classification_report, confusion_matrix, roc_auc_score,
)

# --- Logistic Regression ---
model = LogisticRegression(max_iter=1000, random_state=42)
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
y_prob = model.predict_proba(X_test)[:, 1]     # probability of class 1

# --- Random Forest Classifier ---
model = RandomForestClassifier(n_estimators=200, max_depth=10, random_state=42, n_jobs=-1)
model.fit(X_train, y_train)

# --- Evaluation ---
print(classification_report(y_test, y_pred))

cm = confusion_matrix(y_test, y_pred)
sns.heatmap(cm, annot=True, fmt="d", cmap="Blues")

print(f"Accuracy:  {accuracy_score(y_test, y_pred):.3f}")
print(f"Precision: {precision_score(y_test, y_pred, average='weighted'):.3f}")
print(f"Recall:    {recall_score(y_test, y_pred, average='weighted'):.3f}")
print(f"F1:        {f1_score(y_test, y_pred, average='weighted'):.3f}")
print(f"AUC-ROC:   {roc_auc_score(y_test, y_prob):.3f}")

# --- ROC Curve ---
from sklearn.metrics import RocCurveDisplay
RocCurveDisplay.from_estimator(model, X_test, y_test)
plt.show()

# --- XGBoost ---
from xgboost import XGBClassifier
model = XGBClassifier(n_estimators=200, learning_rate=0.1, max_depth=6,
                       use_label_encoder=False, eval_metric="logloss", random_state=42)
model.fit(X_train, y_train, eval_set=[(X_test, y_test)], verbose=False)

# --- LightGBM ---
from lightgbm import LGBMClassifier
model = LGBMClassifier(n_estimators=200, learning_rate=0.1, max_depth=6, random_state=42)
model.fit(X_train, y_train)
```

---

## 43. Clustering

```python
from sklearn.cluster import KMeans, DBSCAN, AgglomerativeClustering
from sklearn.metrics import silhouette_score
from sklearn.preprocessing import StandardScaler

# always scale before clustering
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# --- K-Means ---
kmeans = KMeans(n_clusters=4, random_state=42, n_init=10)
df["cluster"] = kmeans.fit_predict(X_scaled)

print(f"Silhouette: {silhouette_score(X_scaled, df['cluster']):.3f}")
print(f"Inertia: {kmeans.inertia_:.0f}")

# elbow method (find optimal k)
inertias = []
for k in range(2, 11):
    km = KMeans(n_clusters=k, random_state=42, n_init=10)
    km.fit(X_scaled)
    inertias.append(km.inertia_)
plt.plot(range(2, 11), inertias, marker="o")
plt.xlabel("k")
plt.ylabel("Inertia")
plt.title("Elbow Method")
plt.show()

# --- DBSCAN (density-based, finds outliers) ---
dbscan = DBSCAN(eps=0.5, min_samples=5)
df["cluster"] = dbscan.fit_predict(X_scaled)
# label -1 = noise/outlier

# --- Hierarchical ---
from scipy.cluster.hierarchy import dendrogram, linkage
Z = linkage(X_scaled, method="ward")
dendrogram(Z)
plt.show()
```

---

## 44. Model Evaluation Metrics

### Regression Metrics

```python
from sklearn.metrics import (
    mean_squared_error, mean_absolute_error,
    r2_score, mean_absolute_percentage_error,
)

print(f"R²:   {r2_score(y_true, y_pred):.3f}")
print(f"RMSE: {mean_squared_error(y_true, y_pred, squared=False):.2f}")
print(f"MAE:  {mean_absolute_error(y_true, y_pred):.2f}")
print(f"MAPE: {mean_absolute_percentage_error(y_true, y_pred):.2%}")

# adjusted R²
n, p = X_test.shape
r2 = r2_score(y_true, y_pred)
adj_r2 = 1 - (1 - r2) * (n - 1) / (n - p - 1)
```

### Classification Metrics

```python
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score, f1_score,
    roc_auc_score, average_precision_score, log_loss,
    classification_report, confusion_matrix,
    precision_recall_curve, roc_curve,
)

# comprehensive report
print(classification_report(y_true, y_pred, digits=3))

# individual metrics
accuracy  = accuracy_score(y_true, y_pred)
precision = precision_score(y_true, y_pred, average="weighted")
recall    = recall_score(y_true, y_pred, average="weighted")
f1        = f1_score(y_true, y_pred, average="weighted")
auc       = roc_auc_score(y_true, y_prob)

# confusion matrix
cm = confusion_matrix(y_true, y_pred)
tn, fp, fn, tp = cm.ravel()         # for binary
specificity = tn / (tn + fp)
```

### When to Use Which Metric

| Metric | Use When |
|--------|----------|
| Accuracy | Balanced classes |
| Precision | Cost of false positives is high (spam detection) |
| Recall | Cost of false negatives is high (disease detection) |
| F1 | Balance between precision and recall |
| AUC-ROC | Comparing models, probability ranking |
| RMSE | Penalize large errors more |
| MAE | All errors weighted equally |
| MAPE | Need percentage-based interpretation |

---

# Part 9: Big Data and Performance

---

## 45. Pandas Performance Optimization

```python
# --- use vectorized operations, avoid loops ---

# BAD: iterating rows
for i, row in df.iterrows():
    df.loc[i, "result"] = row["a"] + row["b"]

# GOOD: vectorized
df["result"] = df["a"] + df["b"]

# --- use .values or .to_numpy() for speed ---
arr = df["salary"].to_numpy()

# --- use categorical for low-cardinality strings ---
df["department"] = df["department"].astype("category")

# --- use query() for complex filters ---
df.query("salary > 80000 and department == 'Engineering'")

# --- use eval() for column expressions ---
df.eval("annual_salary = salary * 12", inplace=True)

# --- read only needed columns ---
df = pd.read_csv("big.csv", usecols=["id", "name", "salary"])

# --- use appropriate dtypes from the start ---
df = pd.read_csv("data.csv", dtype={"zipcode": str, "count": "int32", "amount": "float32"})

# --- chunk processing for files that don't fit in memory ---
result = []
for chunk in pd.read_csv("huge.csv", chunksize=500_000):
    filtered = chunk[chunk["status"] == "active"]
    agg = filtered.groupby("category")["amount"].sum()
    result.append(agg)
final = pd.concat(result).groupby(level=0).sum()

# --- use parquet instead of CSV ---
# 5-10x faster reads, 2-5x smaller files
df.to_parquet("data.parquet")
df = pd.read_parquet("data.parquet")

# --- benchmark ---
import time
start = time.perf_counter()
# ... operation ...
elapsed = time.perf_counter() - start
print(f"Took {elapsed:.2f}s")

# or
%timeit df["a"] + df["b"]    # Jupyter magic
```

---

## 46. PySpark Essentials

```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from pyspark.sql.window import Window

# --- create session ---
spark = SparkSession.builder \
    .appName("DataPipeline") \
    .config("spark.sql.shuffle.partitions", "200") \
    .getOrCreate()

# --- read data ---
df = spark.read.csv("data.csv", header=True, inferSchema=True)
df = spark.read.parquet("data.parquet")
df = spark.read.json("data.json")

# from Pandas
spark_df = spark.createDataFrame(pandas_df)

# --- inspect ---
df.show(5)
df.printSchema()
df.count()
df.describe().show()
df.columns

# --- select ---
df.select("name", "salary")
df.select(F.col("name"), F.col("salary") * 12)

# --- filter ---
df.filter(F.col("salary") > 80000)
df.filter((F.col("dept") == "Engineering") & (F.col("salary") > 80000))
df.filter(F.col("dept").isin(["Engineering", "Sales"]))
df.filter(F.col("name").like("%Smith%"))
df.filter(F.col("email").isNotNull())

# --- add/rename/drop columns ---
df = df.withColumn("annual", F.col("salary") * 12)
df = df.withColumn("tier", F.when(F.col("salary") > 100000, "Senior")
                             .when(F.col("salary") > 70000, "Mid")
                             .otherwise("Junior"))
df = df.withColumnRenamed("name", "employee_name")
df = df.drop("temp_col")

# --- aggregation ---
df.groupBy("department").agg(
    F.avg("salary").alias("avg_salary"),
    F.count("*").alias("headcount"),
    F.max("salary").alias("max_salary"),
)

# --- joins ---
joined = df1.join(df2, on="customer_id", how="left")
joined = df1.join(df2, df1.cust_id == df2.customer_id, "inner")

# --- window functions ---
w = Window.partitionBy("department").orderBy(F.desc("salary"))
df = df.withColumn("rank", F.row_number().over(w))
df = df.withColumn("dept_avg", F.avg("salary").over(Window.partitionBy("department")))

# --- sort ---
df.orderBy("salary")
df.orderBy(F.desc("salary"))

# --- write ---
df.write.parquet("output.parquet", mode="overwrite")
df.write.partitionBy("year", "month").parquet("output/")
df.write.csv("output.csv", header=True, mode="overwrite")

# --- SQL ---
df.createOrReplaceTempView("employees")
result = spark.sql("SELECT department, AVG(salary) FROM employees GROUP BY department")

# --- to Pandas ---
pandas_df = df.toPandas()

# --- caching ---
df.cache()           # keep in memory
df.unpersist()       # release from memory

# --- UDF (user-defined function) ---
from pyspark.sql.types import StringType

@F.udf(StringType())
def clean_name(name):
    return name.strip().title() if name else None

df = df.withColumn("name_clean", clean_name(F.col("name")))
```

---

## 47. Polars — Fast DataFrame Library

```python
import polars as pl

# --- read ---
df = pl.read_csv("data.csv")
df = pl.read_parquet("data.parquet")

# --- lazy evaluation (optimized execution) ---
lazy = pl.scan_csv("huge.csv")
result = (
    lazy
    .filter(pl.col("salary") > 80000)
    .group_by("department")
    .agg(pl.col("salary").mean().alias("avg_salary"))
    .sort("avg_salary", descending=True)
    .collect()       # execute
)

# --- select ---
df.select("name", "salary")
df.select(pl.col("name"), (pl.col("salary") * 12).alias("annual"))

# --- filter ---
df.filter(pl.col("salary") > 80000)
df.filter((pl.col("dept") == "Engineering") & (pl.col("salary") > 80000))

# --- add columns ---
df = df.with_columns(
    (pl.col("salary") * 12).alias("annual"),
    pl.when(pl.col("salary") > 100000).then(pl.lit("Senior"))
      .when(pl.col("salary") > 70000).then(pl.lit("Mid"))
      .otherwise(pl.lit("Junior")).alias("tier"),
)

# --- group by ---
df.group_by("department").agg(
    pl.col("salary").mean().alias("avg_salary"),
    pl.col("salary").max().alias("max_salary"),
    pl.len().alias("count"),
)

# --- join ---
joined = df1.join(df2, on="customer_id", how="left")

# --- window functions ---
df = df.with_columns(
    pl.col("salary").rank(descending=True).over("department").alias("dept_rank"),
    pl.col("salary").mean().over("department").alias("dept_avg"),
)

# --- sort ---
df.sort("salary", descending=True)

# --- to pandas ---
pandas_df = df.to_pandas()

# --- why polars? ---
# 10-100x faster than pandas for large datasets
# uses all CPU cores
# lazy evaluation optimizes query plans
# lower memory usage (Apache Arrow backend)
```

---

# Part 10: Automation and Pipelines

---

## 48. Scheduling and Automation

### Running Scripts on a Schedule

```python
# --- schedule library ---
import schedule
import time

def daily_report():
    df = pd.read_sql("SELECT ...", engine)
    df.to_excel(f"report_{date.today()}.xlsx")
    print(f"Report generated at {datetime.now()}")

schedule.every().day.at("08:00").do(daily_report)
schedule.every(30).minutes.do(check_data_quality)

while True:
    schedule.run_pending()
    time.sleep(60)
```

### Command-Line Arguments

```python
import argparse

parser = argparse.ArgumentParser(description="Data pipeline")
parser.add_argument("--input", required=True, help="Input file path")
parser.add_argument("--output", default="output.csv", help="Output file path")
parser.add_argument("--date", help="Processing date (YYYY-MM-DD)")
parser.add_argument("--dry-run", action="store_true", help="Don't write output")
args = parser.parse_args()

df = pd.read_csv(args.input)
if not args.dry_run:
    df.to_csv(args.output, index=False)
```

### Simple ETL Pipeline

```python
def extract(source_path):
    """Load raw data."""
    logger.info(f"Extracting from {source_path}")
    return pd.read_csv(source_path)

def transform(df):
    """Clean and transform."""
    logger.info("Transforming data")
    df = clean_dataframe(df)
    df["revenue"] = df["quantity"] * df["price"]
    df["month"] = pd.to_datetime(df["date"]).dt.to_period("M")
    return df

def load(df, target_path):
    """Write output."""
    logger.info(f"Loading to {target_path}")
    df.to_parquet(target_path, index=False)
    logger.info(f"Wrote {len(df)} rows")

# run
raw = extract("data/raw/sales.csv")
clean = transform(raw)
load(clean, "data/processed/sales.parquet")
```

---

## 49. Environment and Dependency Management

```bash
# --- pip + requirements.txt ---
pip install pandas numpy scikit-learn
pip freeze > requirements.txt
pip install -r requirements.txt

# --- virtual environments ---
python -m venv .venv
source .venv/bin/activate        # Linux/Mac
.venv\Scripts\activate           # Windows
deactivate

# --- conda ---
conda create -n data_env python=3.11
conda activate data_env
conda install pandas numpy scikit-learn matplotlib seaborn
conda env export > environment.yml
conda env create -f environment.yml

# --- pyproject.toml (modern standard) ---
# [project]
# name = "my-pipeline"
# dependencies = [
#     "pandas>=2.0",
#     "numpy>=1.24",
#     "scikit-learn>=1.3",
# ]

# --- uv (fast modern package manager) ---
uv init my-project
uv add pandas numpy scikit-learn
uv run python pipeline.py
```

---

## 50. Common Recipes and One-Liners

```python
# --- quick data profiling ---
df.describe(include="all").T
df.dtypes.value_counts()
df.isnull().mean().sort_values(ascending=False)
df.nunique().sort_values()

# --- find columns by pattern ---
df.filter(like="date")             # columns containing "date"
df.filter(regex="^sales_")        # columns starting with "sales_"

# --- memory usage ---
df.memory_usage(deep=True).sum() / 1e6    # total MB

# --- value counts as DataFrame ---
df["status"].value_counts().reset_index(name="count")

# --- crosstab with percentages ---
pd.crosstab(df["dept"], df["level"], normalize="index").round(3)

# --- quick correlation with target ---
df.corrwith(df["target"]).sort_values(ascending=False)

# --- sample stratified ---
df.groupby("label").apply(lambda x: x.sample(min(100, len(x)))).reset_index(drop=True)

# --- explode dict column ---
df = pd.concat([df.drop(columns="metadata"), df["metadata"].apply(pd.Series)], axis=1)

# --- flatten MultiIndex columns ---
df.columns = ["_".join(col).strip("_") for col in df.columns.values]

# --- progress bar for apply ---
from tqdm import tqdm
tqdm.pandas()
df["result"] = df["text"].progress_apply(expensive_function)

# --- parallelize with pandarallel ---
from pandarallel import pandarallel
pandarallel.initialize(progress_bar=True)
df["result"] = df["text"].parallel_apply(expensive_function)

# --- time a cell (Jupyter) ---
%%timeit
df.groupby("dept")["salary"].mean()

# --- suppress warnings ---
import warnings
warnings.filterwarnings("ignore", category=FutureWarning)

# --- set pandas display options ---
pd.set_option("display.max_columns", None)
pd.set_option("display.max_rows", 100)
pd.set_option("display.float_format", "{:,.2f}".format)
pd.set_option("display.max_colwidth", 80)

# --- reset display options ---
pd.reset_option("all")
```

---

## Import Cheat Sheet

```python
# core
import pandas as pd
import numpy as np

# visualization
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px

# ML
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.preprocessing import StandardScaler, LabelEncoder
from sklearn.linear_model import LinearRegression, LogisticRegression
from sklearn.ensemble import RandomForestClassifier, RandomForestRegressor
from sklearn.metrics import accuracy_score, mean_squared_error, classification_report

# dates
from datetime import datetime, timedelta, date

# file / path
from pathlib import Path
import json, csv, os

# utilities
from collections import Counter, defaultdict
from functools import reduce
import re
import logging
```

---

*End of reference. Organized by workflow: load → inspect → clean → transform → analyze → visualize → model → deploy.*
