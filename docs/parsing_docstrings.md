# Using Griffe as a docstring-parsing library

You can use Griffe to parse arbitrary docstrings.
You don't have to load anything through the Griffe loader.
You need to import the [`parse`][griffe.docstrings.parsers.parse] function,
the [`Parser`][griffe.docstrings.parsers.Parser] enumeration,
and the [`Docstring`][griffe.dataclasses.Docstring] class.
Then you can build a `Docstring` instance and call `parse` on it,
choosing the parsing-style to use:

```python
from griffe.dataclasses import Docstring
from griffe.docstrings.parsers import Parser, parse

text = "Hello I'm a docstring!"
docstring = Docstring(text, lineno=1)
parsed = parse(docstring, Parser.google)
```

If you want to take advantage of the parsers ability to fetch
annotations from the object from which the docstring originates,
you can manually create the parent objects and link them to the docstring:

```python
from griffe.dataclasses import Docstring, Function, Parameters, Parameter, ParameterKind
from griffe.docstrings.parsers import Parser, parse

function = Function(
    "func",
    parameters=Parameters(
        Parameter("param1", annotation="str", kind=ParameterKind.positional_or_keyword),
        Parameter("param2", annotation="int", kind=ParameterKind.keyword_only),
    )
)
text = """
Hello I'm a docstring!

Parameters:
    param1: Description.
    param2: Description.
"""
docstring = Docstring(text, lineno=1, parent=function)
parsed = parse(docstring, Parser.google)
```

With this the parser will fetch the `str` and `int` annotations
from the parent function's parameters.
