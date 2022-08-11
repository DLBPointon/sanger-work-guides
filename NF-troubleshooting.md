# Welcome to the troubleshooting guide

These are some of the issues we have come across whilst writing pipelines in NF-core DSL2:

### runScript_closure1

#### Error

```
No signature of method: Script_9979a917.BB_GENERATOR() is applicable for argument types: 
(Script_9979a917$_runScript_closure1) values: [Script_9979a917$_runScript_closure1@7e017f47]
```

#### Solution

In my case it was a spelling mistake, in the 'process' I had spelt process with an extra 's' - 20 mins lost.
