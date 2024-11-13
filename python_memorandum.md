## Import a file in the REPL
Thou can load `python -i <myfile.py>` [//]: # (Both execute content's file and drop into REPL mode)

```python
python -i
import myfile
```

### Reload that loaded file

1. After having applied some modifications to the source code, type the following:
```python
import importlib
importlib.reload(myfile)
```

*N.B.*: beware to not attempting to reload the actual file but the **module object** (*i.e.* without specifying the `.py` extension).

2. In the case where the loaded seems to fail or return an error like `TypeError: reload() argument must be a module`, try out to diagnose whether the file as actually been loaded:

```python
import sys
'myfile' in sys.modules
```

3. Thou can ensure that said file is actually in the current working directory that you are dealing with using that method:
```python
import os
os.getcwd()
```

4. Try out the following to list all loaded modules: `print(sys.modules.keys())`.
Thy file shouldth be appears in that list.

5. Then check out thy list of files in the event that thou miss the correct pathname: `print(os.list('.))`

6. Be warned that when thou hadth already called a function from the module using the REPL, that said name remains in the REPL's namespace even after reloading.

Either re-import the module by typing: `from myfile import <new_function_name>`
Or use the dot notation: `<myfile>.<new_function_name>`