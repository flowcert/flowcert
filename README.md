FlowCert: Anonymized Repo for OOPSLA 2024
---

This project is a translation validation tool for the [RipTide](https://doi.org/10.1109/MICRO56248.2022.00046)
dataflow compiler: given an input LLVM function and the output
dataflow program from the compiler, we check that they are equivalent,
on all possible inputs and schedules of dataflow operators.

Directories in this repo:
- `confluence` contains a mechanized [Verus](https://github.com/verus-lang/verus) formalization of Section 5.1.
- `evaluations` contains our benchmark programs.
- `sdc` contains pre-compiled binaries of the RipTide dataflow compiler.
- `semantics` contains the main source code of FlowCert, including dataflow semantics (`semantics/dataflow`), LLVM semantics (`semantics/llvm`), and the two-phase translation validation algorithm (`semantics/simulation.py`).
- `tools` and `utils` contain some auxiliary tools.

## Prerequisites

Since the RipTide compiler is not yet open-sourced, we prepared pre-compiled
Linux shared binary for ARM64 and x86_64 platforms to be used in the Docker.
So an ARM64 or x86_64 machine is required; the system doesn't have to be Linux, but it should be able to run Docker.

Please follow the [installation guide](https://docs.docker.com/engine/install/) to install Docker 20.10+.

To build the Docker container on your machine (which involves compiling LLVM 12.0.0 and setting up our repo):
```
docker build -t flowcert --build-arg MAKE_JOBS=1 .
```

If the build succeeds, run the following to start the Docker image:
```
docker run -it flowcert
```

## Usage

To run our evaluation, run within the Docker image:
```
cd evaluations && make
```
After this finishes, in the `evaluations` directory, for each benchmark program `<name>`, `<name>.bisim.out` will contain outputs from FlowCert.
