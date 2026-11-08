---
description: Gather facts for a Context Scaffold audit
agent: ask
---
For each file in context/, report: the date it last changed (from git log),
the files and features it names, any other file in context/ that states the same rule or a conflicting one,
and any model, tool, or dependency name it contains.
Present the result as a table with columns File, Last changed, Names, Overlaps or conflicts, Vendor names.
Do not assign verdicts; I will decide Keep, Rewrite, Merge, or Remove.
