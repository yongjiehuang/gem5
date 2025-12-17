# Implementation of Post-fetch Correction in gem5

This branch contains a gem5 implementation of **Post-fetch Correction (PFC)**. This mechanism is described on the paper **"Re-establishing Fetch-Directed Instruction Prefetching: An Industry Perspective"** (ISPASS'21) [1].

## Base Implementation & Acknowledgments
**This work is built upon the [gem5-fdp](https://github.com/dhschall/gem5-fdp) repository.**

The `gem5-fdp` repository provides the foundational **Fetch Directed Prefetching (FDP)** architecture, including:
* **Decoupled Frontend**: Separation of the fetch unit from the branch predictor.
* **Fetch Target Queue (FTQ)**: The structure required to buffer predicted fetch targets.

My contribution in this branch focuses on implementing the **PFC** mechanism on top of this FDP infrastructure to handle BTB misses more efficiently.

## Motivation of PFC
A branch goes undetected on a BTB miss and no prediction is made. This would lead to wrong fetching for taken branches until the branch misprediction is detected. However, modern branch predictors, e.g., TAGE-SC-L, can correctly predict the direction of BTB-miss branches [1]. Based on this observation, PFC records prediction outcome(hint) for every instruction when generating the fetch target. If a BTB-miss conditional branch is identified at decode, redirect the front-end if its hint is taken. The performance improvement comes from the earlier wrong-path redirection at decode instead of at the time when branch outcome is solved. In orther word, this mechanism chooses to believe the highly accuracy of modern branch predictor. 

## How to use
We included an example [script](./configs/example/gem5_library/fdp-hello-pfc.py)
how to configure FDP with PFC enabled correctly and simulate a simple "Hello World!" program.

```bash
# Build gem5
scons build/ALL/gem5.opt
# Run the simulation
./build/ALL/gem5.opt \
    configs/example/gem5_library/fdp-hello-pfc.py \
        --isa X86 --pfc

```

## References
[1] Ishii, Yasuo, et al. "Re-establishing fetch-directed instruction prefetching: An industry perspective." 2021 IEEE International Symposium on Performance Analysis of Systems and Software (ISPASS). IEEE, 2021.


---
---

# The gem5 Simulator

This is the repository for the gem5 simulator. It contains the full source code
for the simulator and all tests and regressions.

The gem5 simulator is a modular platform for computer-system architecture
research, encompassing system-level architecture as well as processor
microarchitecture. It is primarily used to evaluate new hardware designs,
system software changes, and compile-time and run-time system optimizations.

The main website can be found at <http://www.gem5.org>.

## Testing status

**Note**: These regard tests run on the develop branch of gem5:
<https://github.com/gem5/gem5/tree/develop>.

[![Daily Tests](https://github.com/gem5/gem5/actions/workflows/daily-tests.yaml/badge.svg?branch=develop)](https://github.com/gem5/gem5/actions/workflows/daily-tests.yaml)
[![Weekly Tests](https://github.com/gem5/gem5/actions/workflows/weekly-tests.yaml/badge.svg?branch=develop)](https://github.com/gem5/gem5/actions/workflows/weekly-tests.yaml)
[![Compiler Tests](https://github.com/gem5/gem5/actions/workflows/compiler-tests.yaml/badge.svg?branch=develop)](https://github.com/gem5/gem5/actions/workflows/compiler-tests.yaml)

## Getting started

A good starting point is <http://www.gem5.org/about>, and for
more information about building the simulator and getting started
please see <http://www.gem5.org/documentation> and
<http://www.gem5.org/documentation/learning_gem5/introduction>.

## Building gem5

To build gem5, you will need the following software: g++ or clang,
Python (gem5 links in the Python interpreter), SCons, zlib, m4, and lastly
protobuf if you want trace capture and playback support. Please see
<http://www.gem5.org/documentation/general_docs/building> for more details
concerning the minimum versions of these tools.

Once you have all dependencies resolved, execute
`scons build/ALL/gem5.opt` to build an optimized version of the gem5 binary
(`gem5.opt`) containing all gem5 ISAs. If you only wish to compile gem5 to
include a single ISA, you can replace `ALL` with the name of the ISA. Valid
options include `ARM`, `NULL`, `MIPS`, `POWER`, `RISCV`, `SPARC`, and `X86`
The complete list of options can be found in the build_opts directory.

See https://www.gem5.org/documentation/general_docs/building for more
information on building gem5.

## The Source Tree

The main source tree includes these subdirectories:

* build_opts: pre-made default configurations for gem5
* build_tools: tools used internally by gem5's build process.
* configs: example simulation configuration scripts
* ext: less-common external packages needed to build gem5
* include: include files for use in other programs
* site_scons: modular components of the build system
* src: source code of the gem5 simulator. The C++ source, Python wrappers, and Python standard library are found in this directory.
* system: source for some optional system software for simulated systems
* tests: regression tests
* util: useful utility programs and files

## gem5 Resources

To run full-system simulations, you may need compiled system firmware, kernel
binaries and one or more disk images, depending on gem5's configuration and
what type of workload you're trying to run. Many of these resources can be
obtained from <https://resources.gem5.org>.

More information on gem5 Resources can be found at
<https://www.gem5.org/documentation/general_docs/gem5_resources/>.

## Getting Help, Reporting bugs, and Requesting Features

We provide a variety of channels for users and developers to get help, report
bugs, requests features, or engage in community discussions. Below
are a few of the most common we recommend using.

* **GitHub Discussions**: A GitHub Discussions page. This can be used to start
discussions or ask questions. Available at
<https://github.com/orgs/gem5/discussions>.
* **GitHub Issues**: A GitHub Issues page for reporting bugs or requesting
features. Available at <https://github.com/gem5/gem5/issues>.
* **Jira Issue Tracker**: A Jira Issue Tracker for reporting bugs or requesting
features. Available at <https://gem5.atlassian.net/>.
* **Slack**: A Slack server with a variety of channels for the gem5 community
to engage in a variety of discussions. Please visit
<https://www.gem5.org/join-slack> to join.
* **gem5-users@gem5.org**: A mailing list for users of gem5 to ask questions
or start discussions. To join the mailing list please visit
<https://www.gem5.org/mailing_lists>.
* **gem5-dev@gem5.org**: A mailing list for developers of gem5 to ask questions
or start discussions. To join the mailing list please visit
<https://www.gem5.org/mailing_lists>.

## Contributing to gem5

We hope you enjoy using gem5. When appropriate we advise sharing your
contributions to the project. <https://www.gem5.org/contributing> can help you
get started. Additional information can be found in the CONTRIBUTING.md file.
