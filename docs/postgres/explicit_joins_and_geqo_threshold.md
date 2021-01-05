# Explicit Joins and Genetic Query Optimization
While debugging a hairy, slow query, I noticed that a difference in `JOIN` and conditons in the `WHERE` clause resulted in significantly different query times. How does this happen; isn't the query planner supposed to be smart?

I read the [Explicit `JOIN` Clauses doc](https://www.postgresql.org/docs/current/explicit-joins.html) and learned the following:

> When a query only involves two or three tables, there aren't many join orders to worry about. But the number of possible join orders grows exponentially as the number of tables expands. Beyond ten or so input tables it's no longer practical to do an exhaustive search of all the possibilities, and even for six or seven tables planning might take an annoyingly long time. When there are too many input tables, the PostgreSQL planner will switch from exhaustive search to a genetic probabilistic search through a limited number of possibilities. (The switch-over threshold is set by the geqo_threshold run-time parameter.) The genetic search takes less time, but it won't necessarily find the best possible plan.

It turns out this particular query involved 13 joins. I need to investigate this further to see if tweaking this parameter really changes things. An alternative and perhaps better approach would be breaking up the query and handling some of the joining elsewhere (python, additional queries etc.)
