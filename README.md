# Static PTX Compiler Python binding and Numba patch

This package provides a Python binding for the `libptxcompiler_static.a` library
and a Numba patch that fixes it to use the static library for compiling PTX
instead of the linker. This enables Numba to support CUDA enhanced
compatibility for scenarios where a single PTX file is compiled and linked as
part of the compilation process. This covers all use cases, except:

- Using Cooperative Groups.
- Debugging features - this includes debug and lineinfo generation, and
  exception checking inside CUDA kernels.


## Numba support

Numba 0.54.1 and the current master branch are supported - other versions of
Numba are unsupported.


## Installation

Install with either:

```
python setup.py develop
```

or

```
python setup.py install
```


## Testing

Run

```
pytest
```

or

```
python ptxcompiler/tests/test_lib.py
```


## Usage

To patch Numba to use the Static PTX Compiler library if needed, call the
`patch_numba_codegen_if_needed()` function:

```python
from ptxcompiler.patch import patch_numba_codegen_if_needed
patch_numba_codegen_if_needed()
```

This function spawns a new process to check the CUDA Driver and Runtime
versions, so it can be safely called at any point in a process. It will only
patch Numba when the Runtime version exceeds the Driver version.


# To do

- Better test coverage
- Add testing of the Numba patch function
- Conda packaging
