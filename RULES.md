# Scenario C rules

This project tests Cube Core as a semantic layer over BookSQL without an additional business-context layer.

## Required behavior

1. Use only the `cube_semantic` MCP for BookSQL data. Use `mcp_call` with `server: "cube_semantic"` and `cube_metadata` or `cube_query`. If tool discovery is needed, use `mcp_connect` for this server, then call tools through `mcp_call`.
2. Never use SQLite MCP, Nao's native `execute_sql`, direct SQL, or another data route. If Cube is unavailable, report the failure without fallback.
3. Use only measures, dimensions, and filters returned by Cube metadata. Do not invent fields, joins, accounting policies, sign conventions, currency conversions, or date rules.
4. Preserve Cube-returned values for reasoning. Do not round source rows or silently apply a display/financial rule absent from the question or the semantic model. For derived arithmetic, use `decimal_calculator.calculate` on values already returned by Cube; keep intermediate inputs unrounded and round only a final displayed value when requested.
5. Report the semantic route and relevant filters/time grain. If Cube cannot provide a requested definition/result, state the limitation instead of guessing.
6. For long result lists, include all rows only if they fit; otherwise state that the bounded response is incomplete and use an approved export path rather than silently omitting rows.

## Arithmetic boundary

`decimal_calculator.calculate` is the only allowed calculator and is arithmetic-only. Cube selects, filters, and aggregates data; the calculator evaluates formulas over the values Cube returned. Do not send raw rows to the calculator for database aggregation.

## First-turn response requirement
Always answer the user's question in the current response and provide every requested field; if data is unavailable, state that explicitly without inventing values or deferring the answer to a follow-up.
