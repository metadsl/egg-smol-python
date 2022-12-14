# How does egg-smol handle boolean values?

Some functions, like `<<` return an optional int.
Others, like `<` return an optional unit.

How do these get translated into the type system of the language?

Are optional values automatically unwrapped?

It looks like we can use them as conditionals as well, like this `(check (< 1 2))`.

Let's look at how check will be implemented in egg-smol:

1. The rule for parsing `check` in `parse.lalrpop` is:

   ```
   "(" "check" <Fact> ")" => Command::Check(<>)
   ```

2. The `Command::Check` command is handled in `run_command` to call `check_fact`.
3. If the fact is an expression, it has to be a call which returns the `Unit` sort.

Let's see what happens if we call check on something that is not true:

```python
from egg_smol.bindings import *
egraph = EGraph()
egraph.check_fact(Fact(
    Call(
        "<",
        [Lit(Int(2)), Lit(Int(1))]
    )

))
```

We get:

```
PanicException: prim was partial... do we allow this?
```

And what if we call check with a non-unit value?

```python
from egg_smol.bindings import *
egraph = EGraph()
egraph.check_fact(Fact(
    Call(
        "+",
        [Lit(Int(2)), Lit(Int(1))]
    )

))
```

Yep, it fails on us:

```
Type mismatch: expr = (+ 2 1), expected = Unit, actual = i64, reason: mismatch
```

So there is not Bool type, there is only `Option<Unit>`. 🤷‍♀️
