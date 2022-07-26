## PreQL

Preql [prequel] is a declarative, typed language that compiles to other data access languages,
most commonly SQL [sequel].


## Why?

SQL is fantastic.

SQL has been the de-facto language for working with data for decades. Data professionals 
can use a common, declarative syntax to interact with anything from local file based databases
to global distributed compute clusters.

SQL solves the wrong problem.

SQL is a declarative language for reading and manipulating data in tables in SQL databases.

This is a perfect fit for an application interacting with a datastore. 

But in data warehouses, a _table_ is an leaky abstraction. Users actually want to declare
_conceptual_ queries. Seeing revenue by product line is a goal; the table that contains
the products and the table that contains revenue are implementation details.

This mismatch between what is being declared
leads to a sprawling mass of SQL that is duplicative an hard to follow and is critical 
to business function. Fortune 500 companies spend millions of dollars trying to reverse 
engineer the original intent of SQL to document dataflow or lineage, or to refactor
business logic when moving to a new database.

## How PreQL solves this

PreQL separates declared conceptual manipulation - [Profit] = [Revenue] - [Cost] from the
database that implements those concepts and executes queries. This allows conceptual 
manipulation to be strongly typed and validated through static analysis, which can then be 
explicitly tested against the declared concrete datasources to produce empirical
proof of correctness for a given expression of business logic. These two definitions can then 
be independently evolved and validated over time. 

A new analyst in a company can be productive in PreQL in minutes; a new analyst with SQL might
take days to understand which tables are relevant. 

#### SQL
```sql
USE AdventureWorks;

SELECT 
    t.Name, 
    SUM(s.SubTotal) AS [Sub Total],
    STR(Sum([TaxAmt])) AS [Total Taxes],
    STR(Sum([TotalDue])) AS [Total Sales]
FROM Sales.SalesOrderHeader AS s
    INNER JOIN Sales.SalesTerritory as t ON s.TerritoryID = t.TerritoryID
GROUP BY 
    t.Name
ORDER BY 
    t.Name
```

### PreQL
```sql
import concepts.sales as sales;

select
    sales.territory_name,
    sales.sub_total,
    sales.total_taxes,
    sales.total_sales,
order by
    sales.territory_name desc;
```


## What's wrong with existing semantic tooling?

Existing semantic models - such as SSAS/OLAP, looker, and data catalogs - require tightly coupling of
conceptual declarations [profit = revenue-cost] to the _instantiated_ instances of those in a specific
model or database. This inevitably leads to redeclaration and duplication and large migration costs.

By declaring those in a loosely coupled format, it is possible to avoid much of the 'refactoring' cost these tools 
can incur. It's significantly simpler to define a concept "once" and have that definition follow each
defined table/datasource that implements it. 

In fact a preql model could easily be compiled into a format that could be ingested by these tools,
enabling users to still get full value (in an easier to mantain way) and supporting incremental adoption. A preql
model could be directly compiled to SQL or another intermediate semantic model. 

## Getting Started

Preql is currently implemented in an experimental python engine. The language and syntax are evolving, and the
best location to try it is the `preql-playground` found (to be linked).
