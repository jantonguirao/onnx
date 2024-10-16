<!--- SPDX-License-Identifier: Apache-2.0 -->

# Development

To build ONNX from source please follow the instructions listed [here](https://github.com/onnx/onnx#build-onnx-from-source).

Then, after you have made changes to Python and C++ files:

- `Python files`: the changes are effective immediately in your installation. You don't need to install these again.
- `C++ files`: you need to install these again to trigger the native extension build.

Assuming build succeed in the initial step, simply running
```
pip install -e .
```
from onnx root dir should work.

## Folder structure

- `onnx/`: the main folder that all code lies under
  - `onnx.proto`: the protobuf that contains all the structures
  - `checker.py`: a utility to check whether a serialized ONNX proto is legal
  - `shape_inference.py`: a utility to infer types and shapes for ONNX models
  - `version_converter.py`: a utility to upgrade or downgrade version for ONNX models
  - `parser.py`: a utility to create an ONNX model or graph from a textual representation
  - `hub.py`: a utility for downloading models from [ONNX Model Zoo](https://github.com/onnx/models)
  - `compose.py`: a utility to merge ONNX models
  - `helper.py`: tools for graph operation
  - `defs/`: a subfolder that defines the ONNX operators
  - `test/`: test files

## Generated operator documentation

[Operator docs in Operators.md](Operators.md) are automatically generated based on C++ operator definitions and backend Python snippets. To refresh these docs, run the following commands from the repo root and commit the results. Note `ONNX_ML=0` updates Operators.md whereas `ONNX_ML=1` updates Operators-ml.md:

```
set ONNX_ML=0
pip install setup.py
python onnx/defs/gen_doc.py
```

## Adding a new operator

ONNX is an open standard, and we encourage developers to contribute high
quality operators to ONNX specification.
Before proposing a new operator, please read [the tutorial](AddNewOp.md).

# Testing

ONNX uses [pytest](https://docs.pytest.org) as a test driver. To run tests, you'll first need to install pytest:

```
pip install pytest nbval
```

After installing pytest, run from the root of the repo:

```
pytest
```

to begin the tests.

You'll need to regenerate test coverage too, by running this command from the root of the repo:

```
python onnx\backend\test\stat_coverage.py
```

# Static typing (mypy)

We use [mypy](http://mypy-lang.org/) to run static type checks on the onnx code base. To check that your code passes, you'll first need to install the mypy type checker. If you're using python 3, call from your onnx source folder:

```
pip install -e .[mypy]
```

The type checker cannot run in a python 2 environment (but it will check python 2 code).
If you're using python 2, you need to install mypy into your system packages instead:

```
pip3 install mypy==[version]
```
*Note: You'll find the version we're currently using in `setup.py`.*

After having installed mypy, you can run the type checks:

```
python setup.py typecheck
```


# Other developer documentation

* [How to implement ONNX backend (ONNX to something converter)](ImplementingAnOnnxBackend.md)
* [Backend test infrastructure and how to add tests](OnnxBackendTest.md)

# License

[Apache License v2.0](https://github.com/onnx/onnx/blob/master/LICENSE)

# Code of Conduct

[ONNX Open Source Code of Conduct](http://onnx.ai/codeofconduct.html)
