# pyo3-linalg-example

Example for using [ndaray-linalg][linalg] with [PyO3][], and packaging using [maturin][] into wheel

[linalg]: https://github.com/rust-ndarray/ndarray-linalg
[PyO3]: https://github.com/PyO3/pyo3
[maturin]: https://github.com/PyO3/maturin

Q & A
-------

### Cannot find OpenBLAS when `import` extension in Python interpreter

```
ImportError: libopenblas.so.0: cannot open shared object file: No such file or directory
```
Please link OpenBLAS statically

```toml
[dependencies.ndarray-linalg]
version = "0.12.0"
features = ["static"]
```

### Cannot build wheel with manylinux

```
💥 maturin failed
Caused by: Failed to ensure manylinux compliance
Caused by: Your library is not manylinux compliant because it links the following forbidden libraries: ["libgfortran.so.5"]
```

It is not supported yet. Please use `maturin build --manylinux=off`

### Cannot find `libgfortran.so.5` when importing

```
ImportError: libgfortran.so.5: cannot open shared object file: No such file or directory
```
The compiled wheel does not includes `gfortran` (due to OpenBLAS dependency).
You need to install gfortran, e.g.
```
sudo apt install gfortran
```
or equivalent way.

### Compiling OpenBLAS is toooooooo heavy

Static linking in [ndarray-linalg][linalg] (0.12.0) is experimental and only supported for OpenBLAS.
