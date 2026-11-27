# Scenario C rules

This project tests Cube Core as a semantic layer over BookSQL without an additional business-context layer.

## Required behavior

1. Use only the `cube_semantic` MCP for BookSQL data. Use `mcp_call` with `server: "cube_semantic"` and `cube_metadata` or `cube_query`. If tool discovery is needed, use `mcp_connect` for this server, then call tools through `mcp_call`.
2. Never use SQLite MCP, Nao's native `execute_sql`, direct SQL, or another data route. If Cube is unavailable, report the failure without fallback.
3. Use only measures, dimensions, and filters returned by Cube metadata. Do not invent fields, joins, accounting policies, sign conventions, currency conversions, or date rules; for relative dates, follow the shared temporal reference below.
4. Preserve Cube-returned values for reasoning. Do not round source rows or silently apply a display/financial rule absent from the question or the semantic model. For derived arithmetic, use `decimal_calculator.calculate` on values already returned by Cube; keep intermediate inputs unrounded and round only a final displayed value when requested.
5. Report the semantic route and relevant filters/time grain. If Cube cannot provide a requested definition/result, state the limitation instead of guessing.
6. For long result lists, include all rows only if they fit; otherwise state that the bounded response is incomplete and use an approved export path rather than silently omitting rows.

## Arithmetic boundary

`decimal_calculator.calculate` is the only allowed calculator and is arithmetic-only. Cube selects, filters, and aggregates data; the calculator evaluates formulas over the values Cube returned. Do not send raw rows to the calculator for database aggregation.

## Shared BookSQL benchmark temporal reference

For BookSQL benchmark questions, use `2022-06-01` as the fixed `as_of_date`, matching the date used by the official evaluator for its supported temporal replacements. This does not mean every date/time SQL expression is normalized. The anchor overrides the actual system/database clock only for interpreting temporal expressions in this benchmark.

Use this date only to interpret relative time wording or query predicates. It is not a data cutoff: do not add a blanket `transaction_date <= '2022-06-01'`, do not suppress later transactions, and do not add a date filter when the question has no temporal condition. Preserve the original questions, Gold SQL, and source data.

When forming a query for a relative period, resolve it to explicit absolute ISO dates from this anchor. Preserve the question's intended period and the Gold predicate's boundary semantics; distinguish a full calendar period (for example, “this month”) from a period-to-date (for example, “month to date”). For Cube, use absolute date-range endpoints rather than runtime-relative ranges. For SQL, do not rely on `CURRENT_DATE` or `now` at query time. If an evaluator must normalize unchanged Gold SQL, normalize a separate execution copy and use the same anchor for Gold and predictions. Temporal normalization must not alter text literals or other query semantics.

This is a date-only reference; it does not define a time of day or timezone for `CURRENT_TIMESTAMP`/`now`. Do not invent those values unless the benchmark protocol separately specifies them.

## First-turn response requirement
Always answer the user's question in the current response and provide every requested field; if data is unavailable, state that explicitly without inventing values or deferring the answer to a follow-up.
