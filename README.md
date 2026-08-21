# std-make-lib

Shared GNU make infrastructure for the C++ and RPM projects.

The public make-level API is versioned by `STD_MAKE_LIB_VERSION`; the initial
release is `1.0.0` and is tagged `v1.0.0`.

## Project integration

Add this repository as a Git submodule and set project parameters before the
include:

```make
STD_PROFILE := cxx-apps
USE_EL1 := 1
WITH_GTEST := 1

PROGRAM_IDS := MAIN
PROGRAM_MAIN_NAME := example
PROGRAM_MAIN_SOURCES := main.cpp worker.cpp

include submodules/std-make-lib/Makefile.common
```

Project-specific targets and additional prerequisites may be defined after the
include. Variables that are intentionally extended after the include, such as
`LDLIBS`, are referenced lazily by the generated rules.

## Profiles and shared facilities

- `STD_PROFILE=cxx-apps`: parameter-driven release/debug executables and tests.
- `STD_PROFILE=subdirs`: top-level subdirectory orchestration, used by SPIhome.
- `STD_PROFILE=custom`: only common facilities; used by the specialized `el1`
  library build.
- `STD_PROFILE=package-only`: RPM/deploy helpers without C++ build rules.
- `USE_EL1=1`: local/system `el1` discovery, version detection, rebuild stamp,
  and Jenkins `EL1_RPM_DIR` bootstrap support.
- `WITH_GTEST=1`: shared GoogleTest CMake bootstrap.
- `RPM_PACKAGE_IDS`: parameter-driven `easy-rpm`, deploy and optional generic CI
  handling.

The common defaults intentionally follow the `el1` Makefile conventions:
`clang++`, release/debug output trees, `lld`, dependency files, optional
`x86-64-v2`, and `gen/jenkins` CI artifact locations.
