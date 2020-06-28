# unicron

PySpark custom transformation runner that manages function order dependencies with a DAG.

See [this video](https://www.youtube.com/watch?v=hzNsOGt3bHk) for more info about Unicron, a "[god of chaos who devours realities](https://en.wikipedia.org/wiki/Unicron)".

## Public interface

Suppose you have three transformations that append columns to a DataFrame:

* `with_col_a` appends `col_a`
* `with_col_ab` appends `col_ab`
* `with_col_abc` appends `col_abc`

These functions are order dependent so you need to run `with_col_a` first, then `with_col_ab`, then `with_col_abc`.

Suppose an end user wants `col_abc` appended to the DataFrame and they don't want to concern themselves with running all the transformations in a specific order.

unicron lets users specify the columns they want appended to a DataFrame and is responsible for running the required transforations in the correct order.  This is a better user interface for programmers to provide for less technical Spark users.

```python
import unicron

# unicron will intelligently run with_col_a, with_col_ab, and with_col_abc in the right order
# to append col_abc to the DataFrame
new_df = unicron.transform(df, dag, cols = ["col_abc"])

# unicron provides a list of all the columns it can append
unicron.list_cols(dag) # [col_a, col_ab, col_abc]

# unicron can validate that a custom transformation DAG is valid
unicron.validate_dag(dag) # throws an error if the DAG isn't structured logically
```

## todo

* add the PySpark dependency
* create GitHub repo
