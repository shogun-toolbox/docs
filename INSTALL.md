Compiling manually
==================

In case none of the binary packages listed on our website work for your system, or you want to modify Shogun, you will need to build it from source.

##Requirements
The standard GNU/Linux tools and Python are minimal requirements to compile Shogun.
To compile the interfaces, in addition to [swig](http://www.swig.org/) itself, you will need language specific development packages installed, see below.

There is a larger number of optional requirements. The output of cmake output lists
optional dependencies that were found and not found.
If a particular Shogun class is unavailable, this is likely due to an unmet dependency.
See our [docker configuration file](https://github.com/shogun-toolbox/shogun/blob/develop/configs/shogun/Dockerfile) for an example configuration used in our test builds.

You need at least 1 Gigabytes free disk space. If you compile any interface, roughly 4 Gigabytes RAM are need (we are working on reducing this). [CCache](https://ccache.samba.org/) will massively speed up the compilation process (enabled by default).

##Basics
Shogun uses [CMake](https://cmake.org/) for its build. The general workflow is

 * Download source code, or (here) clone the latest develop code. Potentially update submodules.
```
git clone https://github.com/shogun-toolbox/shogun.git
git submodule update --init
```
    
 * Go to the repository root. For example
```
cd shogun
```
    
 * Create the build directory
```
mkdir build
```
    
 * Configure cmake, from the build directory, passing the Shogun source root as argument.
It is recommended to use any of CMake GUIs (e.g. replace `cmake ..` with `ccmake ..`),
in particular if you feel unsure about possible parameters and configurations.
Note that all cmake options read as `-DOPTION=VALUE`.
```
cd build
cmake [options] ..
```
    
 * Compile
```
make
```
    
 * Install (prepend `sudo` if installing system wide)
```
make install
```

Sometimes you might need to clean up your build (e.g. in case of some major
changes). First, try
```
make clean
```

If that does not help, try removing the build directory and starting from scratch afterwards
```
rm -rf build
```

If you prefer to not run the `sudo make install` command system wide, you can
either install Shogun to a custom location (`-DCMAKE_INSTALL_PREFIX=/custom/path`, defaults to `/usr/local`), or even skip `make install` at all.
In both cases, it is necessary to set a number of system libraries for using Shogun,
see [doc/readme/INTERFACES.md](https://github.com/shogun-toolbox/docs/blob/master/INTERFACES.md).

##Interfaces
The native C++ interface is always included.
The cmake options for building interfaces are `-DPythonModular -DOctaveModular -DRModular -DJavaModular -DRubyModular -DLuaModular -DCSharpModular` etc. For example, replace the cmake step above by

    $ cmake -DPythonModular=ON ..

The required packages (here debian/Ubuntu package names) for each interface are
 * Python
   - `python-dev python-numpy`
 * Octave
   - `octave liboctave-dev`
 * R
   - `r-base-core`
 * Java
   - `oracle-java8-installer`, non-standard, e.g. `https://launchpad.net/~webupd8team/+archive/ubuntu`/java
   - `jblas`, a third party library, `https://mikiobraun.github.io/jblas/`
 * Ruby
   - `ruby ruby-dev`
 * Lua
   - `lua5.1 liblua5.1-0-dev`
 * C-Sharp
   - `mono-devel mono-gmcs cli-common-dev`

To *use* the interfaces, in particular if not installing to the default system-wide location, see [doc/readme/INTERFACES.md](https://github.com/shogun-toolbox/docs/blob/master/INTERFACES.md).
See below for how to create the examples from the website locally.

##Generating examples
All Shogun examples at our website are automatically generated code. You can
generate them locally as
```
make meta_examples
```

This requires [PLY for Python](https://pypi.python.org/pypi/ply), package `python-ply`. Both source code and executables (for C++ and all enabled compiled
interface languages like Java, C-Sharp) are created in `build/examples/meta/` when running
`make`.

See [doc/readme/EXAMPLES.md](https://github.com/shogun-toolbox/docs/blob/master/EXAMPLES.md) for details on the examples.

##Problems
In case header files or libraries are not at standard locations one needs
to manually adjust the libray and include paths, `-DCMAKE_INCLUDE_PATH=/my/include/path` and `-DCMAKE_LIBRARY_PATH=/my/library/path`.
A good reference for that is [CMake_Useful_Variables](http://cmake.org/Wiki/CMake_Useful_Variables).

## Got stuck? Found a bug? Need help?
In case you have a problem building Shogun, please open an [issue on github](https://github.com/shogun-toolbox/shogun/issues) with your system details, *exact* commands used, and logs posted as a [gist](https://gist.github.com/).
