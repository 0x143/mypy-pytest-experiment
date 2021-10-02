## If input is checked, failures are reported

```
MYPYPATH=$PWD pytest --mypy-ini-file mypy.ini  tests
```

```
============================= test session starts ==============================
platform linux -- Python 3.9.2, pytest-6.2.5, py-1.10.0, pluggy-1.0.0
rootdir: /tmp/mypy-pytest-experiment
plugins: mypy-plugins-1.9.1
collected 2 items

tests_not_failing/test_call.yaml .                                       [ 50%]
tests_not_failing/test_import.yaml .                                     [100%]

============================== 2 passed in 0.64s ===============================
============================= test session starts ==============================
platform linux -- Python 3.9.2, pytest-6.2.5, py-1.10.0, pluggy-1.0.0
rootdir: /tmp/mypy-pytest-experiment
plugins: mypy-plugins-1.9.1
collected 2 items

tests/test_call.yaml F                                                   [ 50%]
tests/test_import.yaml F                                                 [100%]

=================================== FAILURES ===================================
__________________________________ test_call ___________________________________
/tmp/mypy-pytest-experiment/tests/test_call.yaml:6: 
E   pytest_mypy_plugins.utils.TypecheckAssertionError: Invalid output: 
E   Expected:
E     <45 (diff)
E   Actual:
E     main:2: error: Argument 1 to "bar" has incompatible type "str"; expected "int" (diff)
E     main:2: note: Revealed type is "None"         (diff)
E   
E   Alignment of first line difference:
E     E: main:2: note: Revealed type is "None"...
E     A: main:2: error: Argument 1 to "bar" has incompatible type "str"; expected...
E                ^
_________________________________ test_import __________________________________
/tmp/mypy-pytest-experiment/tests/test_import.yaml:6: 
E   pytest_mypy_plugins.utils.TypecheckAssertionError: Invalid output: 
E   Expected:
E     <45 (diff)
E   Actual:
E     main:1: error: Module "foobar" has no attribute "foo" (diff)
E     main:2: note: Revealed type is "Any"          (diff)
E   
E   Alignment of first line difference:
E     E: main:2: note: Revealed type is "def (x: builtins.int)"
E     A: main:1: error: Module "foobar" has no attribute "foo"
E             ^
=========================== short test summary info ============================
FAILED tests/test_call.yaml::test_call - 
FAILED tests/test_import.yaml::test_import - 
============================== 2 failed in 0.76s ===============================
```

## If outputs are not checked, failures are ignored


```
MYPYPATH=$PWD pytest --mypy-ini-file mypy.ini  tests_not_failing
```



```
============================= test session starts ==============================
platform linux -- Python 3.9.2, pytest-6.2.5, py-1.10.0, pluggy-1.0.0
rootdir: /tmp/mypy-pytest-experiment
plugins: mypy-plugins-1.9.1
collected 2 items

tests_not_failing/test_call.yaml .                                       [ 50%]
tests_not_failing/test_import.yaml .                                     [100%]

============================== 2 passed in 0.59s ===============================
```

## mypy called directly, shows errors


```
MYPYPATH=$PWD mypy --config-file mypy.ini tests_py
```


```
tests_py/test_import.py:4: error: Module "foobar" has no attribute "foo"
tests_py/test_call.py:6: error: Argument 1 to "bar" has incompatible type "str"; expected "int"
Found 2 errors in 2 files (checked 2 source files)
```
