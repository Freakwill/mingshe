# MíngShé

A better [Python](https://www.python.org/) superset language. Use [Pegen](https://github.com/we-like-parsers/pegen) to compile the code.

> “鲜山多金玉，无草木，鲜水出焉，而北流注于伊水。其中多鸣蛇，其状如蛇而四翼，其音如磬，见则其邑大旱”——《山海经》

## Install

```
pip install mingshe
```

## Usage

### As a script

Write some code in `main.she`, then run `mingshe ./main.she` to execute code.

```python
10 |> range |> map(pow(?, 2), ?) |> list |> print
```

### Compile to python

You can use `mingshe --compile ./main.she` to generate python file.

### As a module

You can directly import MíngShé script as a module, as long as the script name ends with `.she`.

Example:

```python
# lib.she
def digit_sum(s: str) -> int:
    return s |> map(int, ?) |> sum
```

```python
# main.py
from lib import digit_sum
print(digit_sum('123456'))
```

## Extended syntax

### Pipe

Example:

```
range(10) |> sum |> print
```

Compile to:

```python
print(sum(range(10)))
```

#### Priority

The priority of `|>` is lower than `|` and higher than comparison operations (`in`, `not in`, `is`, `is not`, `<`, `<=`, `>`, `>=`, `!=`, `==`).

### Conditional

Example:

```
a ? b : c
```

Compile to:

```python
b if a else c
```

#### Priority

`a ? b: c` has the same priority as `b if a else c`.

### Partial

Example:

```
square = pow(?, 2)
```

Compile to:

```python
(lambda pow: lambda _0: pow(_0, 2))(pow)
```

### Nullish coalescing

Example:

```
a ?? b
```

Compile to:

```python
a if a is not None else b
```

#### Priority

`??` has the same priority as `or`.

#### Note

When you need to use `??` with `or`, you need to use parentheses in the outer layer, otherwise it will cause a syntax error.

```
a or b ?? c    # syntax error
(a or b) ?? c  # OK
a or (b ?? c)  # OK
```

### Optional chaining

Example:

```
a?.b
```

Compile to:

```python
a if a is None else a.b
```

Example:

```
a?[b]
```

Compile to:

```python
a if a is None else a[b]
```

Example:

```
a?.b()
```

Compile to:

```python
a if a is None else a.b()
```

## Change log

Read [releases](https://github.com/abersheeran/mingshe/releases) to see the change log.
