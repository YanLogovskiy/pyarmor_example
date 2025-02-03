### Dummy package for testing pyarmor obfuscation
1. Install pyarmor
```shell
pip install pyarmor
```

2. Applying it to a package

Instruction is taken from [docs](https://pyarmor.readthedocs.io/en/latest/tutorial/getting-started.html#obfuscating-one-package)

```shell
pyarmor g -O ira/example/ ira/example/*.py
```

3. Check that main.py works fine (it must print 1)
```commandline
python ira/example/main.py
```
output
```commandline
1
```
as it was expected.

4. Try to install package
```commandline
pip install .
```
output
```commandline
        File "/home/yan/ira-labs/example/ira/example/__version__.py", line 2, in <module>
          from pyarmor_runtime_000000 import __pyarmor__
      ModuleNotFoundError: No module named 'pyarmor_runtime_000000'
```
5. Ok, let's try not to install, but providing an extra path to python interpreter.
```commandline
export PYTHONPATH=./
```
And then run tests: with original source code it works fine, but with obfuscated code we get such
output:
```commandline
tests/test_main.py:1: in <module>
    from ira.example import return_one
ira/example/__init__.py:2: in <module>
    from pyarmor_runtime_000000 import __pyarmor__
E   ModuleNotFoundError: No module named 'pyarmor_runtime_000000'

```

#### You can obfuscate several scripts from in your package, that are most important.
But before, ensure that you removed all relative imports from your script, because it will not work.
You need to run pyarmor using the same python version as you app is using.
Like this:
```shell
python /homes/yanlogovskiy/miniconda3/bin/pyarmor g <script_to_obfuscate> [docs](https://pyarmor.readthedocs.io/en/latest/topic/obfuscated-script.html)
```

#### Another way of obfuscation
There is another mode pyarmor can work in: [RFT](https://pyarmor.readthedocs.io/en/latest/tutorial/advanced.html#using-rftmode-pro)
```markdown
RFT mode could rename most of builtins, functions, classes, local variables. 
It equals rewriting scripts in source level.
```
But it works only with script, as I understand from docs. 
So, if we apply it to separate files of project, it will break all relative imports as it will rename variables independently.


#### Conclusion
Pyarmor is good at obfuscating scripts, but it is hard to apply it to project.
It is an example of applying it to super simple package, and even in this case it seems to be quite challenging, 
and I failed to do this.
Considering large production package it might be overcomplicated 
and not worth an effort as it cannot protect code from deobfuscation. 
Maybe I missed something, will be glad to receive a feedback.


