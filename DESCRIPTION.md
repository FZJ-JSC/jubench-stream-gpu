# STREAM GPU

## Purpose

The STREAM GPU benchmark is a variant of the STREAM memory benchmark utilizing the memory of a GPU, measuring the sustainable bandwidth. It only uses little to no computation per byte transferred to or from memory. The benchmark used is written in CUDA.

## Source

Archive name: `stream-gpu-bench.tar.gz`.

The source code is included in the archive. It is equivalent to [commit e0b6c29e](https://github.com/AndiH/CUDA-Cpp-STREAM/commit/e0b6c29ef125540b1c82fa5070206bd77dad2a89) of the [upstream repository on GitHub](https://github.com/AndiH/CUDA-Cpp-STREAM).

## Building

A `Makefile` is provided to ease compilation. CUDA must be available. The `Makefile` executes the following compilation instructions.

```
cd src/CUDA-Cpp-STREAM/
nvcc -std=c++11 -ccbin=g++ stream.cu -arch=sm_80 -o stream_cu.exe
```

The compiler flags might be modified, see _Modification_ section.

When using the JUBE script, JUBE will build the STREAM executable in the compile step (see below).

## Execution

The executable is called `stream`.

### Execution without JUBE

The STREAM GPU benchmark can be executed by running the executable:

```
./stream --nelements $(( 2**28 )) --blocksize 192
```

### Execution with JUBE

Using JUBE, the benchmark can be executed as follows:

```
cd benchmark/jube
jube run stream.jube.xml [--tag tags...]
jube continue stream_run # wait/repeat until all steps are done
jube result -a stream_run
```

The results can be found in the columns `Copy, Scale, Add, Triad`.

The following tags are available for convenience.

|       Tags       |           Effect               | Default |
|------------------|--------------------------------|---------|
| `varySize`       | `STREAM_ARRAY_SIZE=2^10..2^30` |         |
| `fixedSize`      | `STREAM_ARRAY_SIZE=2^28`       | yes     |
| `s23`            | `module load Stages/2023`      | yes     |
| `s22`            | `module load Stages/2022`      |         |

### Configurations

Candidates are requested to run the following configuration:

| Name |             Description              |
|------|--------------------------------------|
| 2^28 | Bandwidth with an array size of 2^28 |

In addition, a scan of array sizes from 2^15 until the memory is full is to be reported; the JUBE tag `varySize` is prepared for this.

### Modification

The number of threads per block (`--blocksize`/`blocksize`) can be chosen freely. The chosen value is to be reported.

For the scan of array sizes, the array sizes should be changed (command line `--nelements` or JUBE variable `size`).

## Verification

All executions should run without errors. In case of JUBE: status _done_, no errors.

## Results

The benchmark presents measured values in the form of tables for the four STREAM kernels. 

Example JUBE outputs look the following (shortened):

| stage | modules  | Exp | Array [MiB] | blocksize |   Triad   | runtime[sec] |
|-------|----------|-----|-------------|-----------|-----------|--------------|
|  2023 | GCC CUDA |  28 |     2147.48 |       192 | 1284.7572 |          1.0 |

## Commitment

The measured average triad bandwidth for an execution with 2^28 elements is to be reported for the quantitative evaluation (given in MiB/s). In addition, a scan of array sizes from 2^15 until the memory is full is to be reported for further evaluation; for each scan point, the average triad bandwidth should be given.
