## Import a file in the REPL
As a side note, thou can load `python -i <myfile.py>` [//]: # (Both execute content's file and drop into REPL mode). Now type this:

[//]: <> (Execute file's content then remains in interactive mode)
`ipython -i <filename.py>`


### Load a file after having run the enhanced interpreter
1. After having applied some modifications to the source code, type the following:
`%run <filename.py>`

However, ipython will execute the script in a separate temporary namespace, meaning that all your symbols created during
your current session will not be affected by said script.

2. Running `%run -i <filename.py>` will made available thy symbols to the script during its execution.


### Reset thy namespace
Running the following ensures that thou run the script in a clean slate while preserving access to pre-loaded libraries
and symbols created in the previous interactive session.
```python
%reset -f
%run -i <filename.py>
```


### Trouble shooting
1. In the case where the loading seems to having failed or having returned an error like `TypeError: reload() argument \
must be a module`, try out to diagnose whether the file hadth actually been loaded:

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

Apply that solution each time thou adds a new function or change its signature** in the source code.


### Type annotations
Keep in mind that type annotations in Python are merely hints and enforce not any type checking at runtime. Use mypy, pyright or Pytype to validate those annotations.
