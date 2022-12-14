---
file_format: mystnb
---

# How-to guides

## Parsing and running program strings

You can provide your program in a special DSL language and run it with {meth}`egg_smol.bindings.EGraph.parse_and_run_program`:

```{code-cell}
from egg_smol.bindings import EGraph

egraph = EGraph()
egraph.parse_and_run_program("(check (= (+ 1 2) 3))")
```

## Developing this package

To get started developing on this package:

1. Create a Python environment to develop on, either with virtualenv or conda.
2. Install this package in editable mode: `pip install -e .[dev,test]`
3. Run the tests: `pytest`
4. Run the pre-commit hooks: `pre-commit run --all-files`
