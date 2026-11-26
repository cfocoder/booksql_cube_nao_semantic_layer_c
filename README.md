# Scenario C — Cube BookSQL semantic layer without extra business context

This repository is the Nao Multi-project context for BookSQL scenario **C**.

## Boundary

- BookSQL data is accessed only through `cube_semantic` (`cube_metadata` and `cube_query`) and the dedicated Cube BookSQL MCP.
- The physical Cube source is SQLite `accounting.sqlite`; C does not connect to SQLite directly.
- No direct SQL, native database connection, or business-policy skill is present.
- `decimal_calculator.calculate` is available only for arithmetic over values already returned by Cube.
- Use the measures/dimensions exposed by live Cube metadata; do not assume a measure or join absent from that metadata.

## Project mapping

- Nao project: `booksql-condition-c`
- Repository: `cfocoder/booksql_cube_nao_semantic_layer_c`
- Expected route: `cube_semantic` → private Cube MCP → Cube Core → SQLite
