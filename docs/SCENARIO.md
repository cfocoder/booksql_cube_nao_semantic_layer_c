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

C uses only Cube for BookSQL data. The Nao project does not receive `CUBE_API_SECRET` or a direct SQLite/PostgreSQL credential; the Cube MCP service owns the Cube API secret. `cube-mcp-booksql` must be a separate private service on the Coolify network, not the Contoso `cube-mcp-private` service.

## Validation

Verify MCP metadata and a bounded BookSQL query from the actual Nao runtime before using C. Confirm the metadata/model is BookSQL and that no direct database connection is configured in the project.
