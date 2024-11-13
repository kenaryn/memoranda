## Import a file in the REPL
As a side note, thou can load `python -i <myfile.py>` [//]: # (Both execute content's file and drop into REPL mode). Now type this:

```python
python -i
import myfile
```

### Reload that file

1. After having applied some modifications to the source code, type the following:
```python
import importlib
importlib.reload(myfile)
```

*N.B.*: be cautious to not attempt to reload the actual file but the **module object** (*i.e.* without specifying the `.py` extension).

2. In the case where the loading seems to having failed or having returned an error like `TypeError: reload() argument must be a module`, try out to diagnose whether the file hadth actually been loaded:

```python
import sys
'myfile' in sys.modules
```

3. Thou can ensure that said file is actually in the current working directory that you are dealing with invoking that method:
```python
import os
os.getcwd()
```

4. Try out the following to list all loaded modules: `print(sys.modules.keys())`.
Thy file shouldth appear in that list.

5. Then check out thy list of files in the event that thou miss the correct pathname: `print(os.listdir('.'))`

6. Be warned that when thou hadth already called a function from the module using the REPL, that said name remains in the REPL's namespace even after reloading.

That is why thou need to either re-import the module by typing: `from myfile import <new_function_name>` \
Or use the dot notation: `<myfile>.<new_function_name>`

Apply that solution ùùeach time thou adds a new function or change its signature** in the source code.


### Type annotations
Keep in mind that type annotations in Python are merely hints and enforce not any type checking at runtime. Use mypy, pyright or Pytype to validate those annotations.

Thou can postpone the annotation's evaluation to avoid forward references issues and improve the program's start up time:
`from __future__ import annotations`