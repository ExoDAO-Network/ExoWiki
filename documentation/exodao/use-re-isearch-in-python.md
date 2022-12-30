# Use re-Isearch in Python

re-Isearch is implemented mainly in C / C++. We can create a Python Binding using [SWIG](https://www.swig.org) (_Simplified Wrapper and Interface Generator_).

Simply go to the _swig_ folder in your local copy.

```
cd /re-Isearch/swig
```

There, you can edit  `Makefile.py` to specify the Python version you are using in `PYVERSION`.&#x20;

Now simply run

```
make Python
```

This creates `PyIB.so`, which is a _shared object file_ that can be loaded into your Python script.

### Example

```python
from IB import * 
```

