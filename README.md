# argsl️

`argsl` is for everybody who finds `argparse` too verbose.

`argsl` (`args DSL` or `argparse DSL`) allows you to specify your CLI flags
and arguments with a succinct notation 
(and at the same time specifying the `--help` text!).

```python
args = argsl("""
filename <path!>         # required positional
--name <str!>            # required flag 
--debug <flag>           # boolean switch
""")
```

instead of:

```python
# argparse version
parser = argparse.ArgumentParser()
parser.add_argument("filename", type=pathlib.Path)
parser.add_argument("--name", required=True)
parser.add_argument("--debug", action="store_true")
```
---

- ✅ **Expressive**: just describe your arguments
- ✅ **No boilerplate**: get `argparse.Namespace` directly
- ✅ **Typed and validated**: int, float, str, path, bool, choices, etc.
- ✅ **Compatible with argparse**: wraps it internally

---

## Installation

### Using [`uv`](https://github.com/astral-sh/uv) (recommended):
```bash
uv add argsl
```

### Or with `pip`:
```bash
pip install argsl
```

---

## Basic Example

```python
from argsl import argsl

args = argsl("""
filename <path!>                        # required positional
--name|-n <str=env:USER>                # optional with env fallback
--level|-l <choice:low,med,high="med"> # choice with default
--debug|-d <flag>                       # boolean flag
--onlylong <int=1>                      # long-only argument
--no-cache <flag>                       # negate behavior if passed
""")

print(vars(args))
```

In your `pyproject.toml`:
```toml
[project.scripts]
run = "main:main"
```
Run it:
```bash
uv run run file.txt --name Alice --debug --level high --no-cache
```

---

## Supported Argument Types

| DSL Expression               | Description                            |
|-----------------------------|----------------------------------------|
| `<str>`, `<int>`, `<float>` | Single typed argument                  |
| `<path>`                    | File/path via `pathlib.Path`           |
| `<flag>`                    | Boolean flag (false unless present)    |
| `<str!>`                    | Required string argument               |
| `<int=42>`                  | With default value                     |
| `<choice:a,b,c="b">`        | Choice + default                       |
| `<str*>`                    | Accept multiple values                 |
| `=env:VAR`                  | Fallback to environment variable       |

---

## Supported Flag Variants

```python
args = argsl("""
--user|-u <str!>      # short + long
--only <int=1>        # long-only
--debug <flag>        # long-only boolean flag
--no-cache <flag>     # negated logic: default False → True if passed
""")
```

---

## Test Coverage & Edge Cases

✔ Required positional + typed
✔ Required named args (`--foo <str!>`)  
✔ Optional args with defaults (`--bar <int=42>`)  
✔ Multiple values (`--list <str*>`)
✔ Boolean flags (`--debug <flag>`)  
✔ Env fallback (`--x <str=env:HOME>`)  
✔ Quoted defaults (`--title <str="The Answer">`)  
✔ Long-only or short+long (`--onlylong`, `--msg|-m`)
✔ Choice types (`--level <choice:low,med,high="med">`)
✔ All covered in `tests/test_argsl_all.py`

---

## Development Workflow

```bash
uv add .[dev]
uv add '.[dev]'   # on zsh (macos)
uv run test
```

Or with classic tools:
```bash
pip install -e .[dev]
pip install -e '.[dev]'   # on zsh (macos)
pytest
```

---

## Publishing to PyPI

```bash
rm -rf dist/     # necessary - otherwise PyPI would upload old version, too, resulting in an error
uv build
uv publish
```

---

MIT licensed • Built by Gwang-Jin Kim
