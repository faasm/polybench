# PolyBench/C in Faasm

This repository contains a slightly modified [Polybench/C benchmark](
https://sourceforge.net/projects/polybench/) for its use with [Faasm](
https://github.com/faasm/faasm).

In particular, we cross-compile PolyBench/C to WebASsembly, and compare it to
native execution. For the cross-compilation scripts, check the [examples](
https://github.com/faasm/examples/tree/main/tasks/polybench.py) and the
[`CMakeLists.txt`](./CMakeLists.txt) in this repository.

> To run with the WAMR runtime, we need to add an extra include header to
> prevent some WASM memory optimisations. Other than that, the benchmark
> requires no source code changes whatsoever.
> NOTE: annoyingly, C source files seem to require a bit more work to get the
> symbols linked in. Look into why that is the case.

TODO: what benchmarks are not there and why?
