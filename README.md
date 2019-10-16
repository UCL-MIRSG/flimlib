# FLIMLib
[![](https://travis-ci.com/flimlib/flimlib.svg?branch=java-lib)](https://travis-ci.com/flimlib/flimlib "Travis")
[![](https://ci.appveyor.com/api/projects/status/github/flimlib/flimlib?svg=true)](https://ci.appveyor.com/project/scijava/flimlib "AppVeyor")

FLIMLib is a curve fitting library used for Fluorescent Lifetime Imaging or
FLIM. It is developed by Paul Barber and the Advanced Technology Group at the
[Oxford Institute for Radiation Oncology](https://www.oncology.ox.ac.uk/),
University of Oxford, as well as the [Laboratory for Optical and Computational
Instrumentation](https://loci.wisc.edu/) at the University of
Wisconsin-Madison. FLIMLib is used for FLIM functionality in the [Time Resolved
Imaging](https://www.assembla.com/spaces/ATD_TRI/wiki) (TRI2) software, as well
as in the [FLIMJ plugin for ImageJ](https://imagej.net/FLIMJ).

For exponential lifetime fitting there are two core algorithms within FLIMLib:

1. A triple integral method that does a very fast estimate of a single
   exponential lifetime component.
2. A Levenberg-Marquardt algorithm or LMA that uses an iterative,
   least-squares-minimization approach to generate a fit. This works with
   single, double and triple exponential models, as well as stretched
   exponential.

There is also code to perform 'global' analysis over a number of signals
simultaneously (e.g. over an image), where the lifetimes can be considered
constant across the data set, but the amplitudes are allowed to vary for each
signal. There is also a completely generic global analysis function. A third
algorithm is available to perform phasor analysis.

In addition there is a non-negative linear least squares algorithm that is
useful for spectral unmixing in combined spectral-lifetime imaging (SLIM).

The code is written in C89 compatible C and is thread safe for fitting multiple
pixels concurrently. Several files are provided as wrappers to call this
library from Java code: `EcfWrapper.c` and `.h` provide a subset of function
calls used by the FLIMJ plugin for ImageJ.

Additionally, there is wrapper code in `cLibrary.i` to wrap the remaining
external functions in `GCI_Phasor.c` and `EcfGlobal.c`. This code generates
swig wrapper files which enable you to call these functions from Java. They
are generated automatically in Eclipse when the flimlib project is updated.

## See also

* [FLIMLib wiki](https://github.com/flimlib/flimlib/wiki)
* [FLIMLib web site](https://flimlib.github.io/)
* [FLIMLib Doxygen docs](http://code.imagej.net/flimlib/html/)

## Directory contents

* `lib` - pre-compiled dll files for Windows 32 and 64 bit.
* `src` - source files
* `src/main/c` - The source files for the FLIMLib library
* `src/main/cpp` - The C++ include file for a FLIMLib class for use in C++ projects
* `src/flimlib-cmd/c` - The source files for the standalone executable wrapper for the library
* `src/flimlib-cmd/cpp` - The source files for the standalone executable written in C++
* `src/matlab` - Wrapper and example code for use of the library with Matlab
* `test_files` - dat and ini settings file for testing

## Building the source

To build the library and standalone program using maven:

  ```
  mvn clean install
  ```

## Running the standalone executable

1.  Copy the executable to the `test_files` folder for convenience

    ```
    cp target/build/bin/flimlib-cmd ./test_files
    ```

2.  Run the program with the test files

    ```
    cd ./test_files
    ./flimlib-cmd test.ini transient.dat
    ```

## Using from a Java project

To depend on FLIMLib from Maven, simply copy the following to appropriate places in your `pom.xml`:

```xml
<properties>
  <flimlib.version>1.1.0</flimlib.version>
</properties>

<!-- FLIMLib Java interface -->
<dependency>
  <groupId>flimlib</groupId>
  <artifactId>flimlib</artifactId>
  <version>${flimlib.version}</version>
</dependency>
<!-- FLIMLib native binary -->
<dependency>
  <groupId>flimlib</groupId>
  <artifactId>flimlib</artifactId>
  <version>${flimlib.version}</version>
  <classifier>${scijava.natives.classifier}</classifier>
  <!-- Or one of the following if you would like to manually specify the binary platform -->
  <!-- <classifier>native-linux_64</classifier> -->
  <!-- <classifier>native-windows_64</classifier> -->
  <!-- <classifier>native-osx_64</classifier> -->
</dependency>
```

*Note that the native binary is platform-dependent. So you may want to make sure that the `<classifier>` attribute is either automatically detected by the parent `scijava` pom (`${scijava.natives.classifier}`) or manually filled in to match your platform.*
