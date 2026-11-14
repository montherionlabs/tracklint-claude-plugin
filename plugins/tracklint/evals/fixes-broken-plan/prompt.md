---
max_turns: 14
timeout_seconds: 300
allowed_tools: [Read, Glob, Grep, Skill]
---

Este plan de Tracklint no pasa la validación. Arréglalo y explícame en pocas líneas qué estaba mal:

```csv
platform,event,required,rules,count
google analytics 4,purchase,transaction_id,value,currency,value bigger than 0,once
facebook ads,Purchase,value;currency,value > 0,twice
```
