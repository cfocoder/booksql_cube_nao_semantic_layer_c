# Scenario C deployment contract

## Required Nao project

```text
Project: booksql-condition-c
Repository: https://github.com/cfocoder/booksql_cube_nao_semantic_layer_c.git
Branch: main
```

## Data route

```text
Nao project -> cube_semantic MCP -> cube-mcp-booksql:8787/mcp
            -> Cube BookSQL API -> SQLite accounting.sqlite working copy
```

C uses only Cube for BookSQL data. The Nao project receives no database credentials; the Cube MCP service owns the Cube API secret and accesses the SQLite-backed BookSQL model. `cube-mcp-booksql` must be a separate private service on the Coolify network, not the Contoso `cube-mcp-private` service.

## Fiscal-year convention

BookSQL fiscal years run from April 1 through March 31, as implied by the authors' [DIN-SQL example](https://github.com/Exploration-Lab/BookSQL/blob/main/GPT/DIN-SQL.py#L1584). With the benchmark reference date 2022-06-01, “this fiscal year” and “this fiscal year to date” mean 2022-04-01 through 2022-06-01; “last fiscal year” means 2021-04-01 through 2022-03-31. A full FY 2022–23, when explicitly requested, means 2022-04-01 through 2023-03-31.

The validation split has 150 Gold queries worded “this fiscal year to date” whose SQL instead covers the full FY through 2023-03-31. Keep questions and official Gold unchanged; flag these cases separately as Gold/wording inconsistencies.

## Validation

Verify MCP metadata and a bounded BookSQL query from the actual Nao runtime before using C. Confirm the metadata/model is BookSQL and that no direct database connection is configured in the project.
