### Big questions

#### What information you need?
#### How to divide that information into the appropriate tables and columns
#### How those tables relate to each other.

### What is good database design?
<mark style="background: #FF5582A6;">Duplicate information (**redundant data**) is bad</mark>
- Divide your info into *subject-based table* -> reduce redundant data
- Allow [[joining table]] to query the data we need

### Step by step

1. Identify the requirements (feature, data)
2. Conceptual Data Modeling
3. Create [[Entity-Relationship Diagram]]
4. [[Normalize data model]] -> split large tables into smaller one and reduce [[redundant data#database]]
5. Define Tables and Columns (Primary key)
6. Establish relationships (relationship type: **{1-1}**, **{1-n}**, **{n-n}**)
7. [OPTIONAL] implement indexs ([[b-tree]], [[bloom filter]])