# Installing Shogun

For certain systems, we offer pre-built packages of Shogun.
This is the easiest way to start using it.
For other cases, we describe how to build Shogun from source code.


# Quicklinks
 * [Ready-to-install packages](#binaries)
   - [Ubuntu](#ubuntu)
   - [Debian](#debian)
   - [Fedora](#fedora)
   - [MacOS](#mac)
   - [Windows](#windows)
 * [Docker images](#docker)
 * [Integration with interface languages](#language)
  - [Python](#pipy)
 * [Compiling manually](#manual)
   - [Requirements](#manual-requirements)
   - [Basics](#manual-basics)
   - [Interfaces](#manual-interfaces)
   - [Examples](#manual-examples)
   - [Problems](#manual-problems)



## Ready-to-install packages <a name="binaries"></a>

### Ubuntu ppa <a name="ubuntu"></a>
We are working on integrating Shogun with Debian/Ubuntu.
In the meantime, we offer a [prepackaged ppa](https://launchpad.net/~shogun-toolbox/+archive/ubuntu/stable).
These currently do contain the C++ library and Python bindings.
Add this to your system as

    sudo add-apt-repository ppa:shogun-toolbox/stable
    sudo apt-get update

Then, install as

    sudo apt-get install libshogun17

The Python (2) bindings can be installed as

    sudo apt-get install python-shogun

In addition to the latest stable release, we offer [nightly builds](https://launchpad.net/~shogun-toolbox/+archive/ubuntu/nightly) of our development branch.

### Debian <a name="debian"></a>
Latest packages for Debian jessie are available in our own repository at [http://apt.shogun.ml](http://apt.shogun.ml).
We provide both the stable and nightly packages, currenlty only for amd64 architecture.
In order to add the stable packages to your system, simply run the following commands

    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3DD2174ACAB30365
    echo "deb http://apt.shogun.ml/ jessie main" | sudo tee /etc/apt/sources.list.d/shogun-toolbox.list  > /dev/null
    sudo apt-get update

After this just simply install the shogun library

    sudo apt-get install libshogun17

The nightly packages are available in the `nightly` component, i.e. `deb http://apt.shogun.ml/ jessie nightly`

### Fedora <a name="fedora"></a>
Shogun is part of [Fedora 25](https://admin.fedoraproject.org/pkgdb/package/rpms/shogun/).
Install as

    sudo dnf install shogun


### MacOS <a name="mac"></a>
Shogun is part of [homebrew-science](https://github.com/Homebrew/homebrew-science).
Install the latest stable version as

    sudo brew install shogun

### Windows <a name="windows"></a>
Shogun natively compiles under Windows using MSVC, see the [CI build](https://ci.appveyor.com/project/vigsterkr/shogun).
We currently do not support a binary installer.
If you are interested in packaging, documenting, or contributing otherwise, please contact us.

## Docker images <a name="docker"></a>
You can run Shogun in [our own cloud](cloud.shogun.ml) or set up your own using our
[Docker images](https://hub.docker.com/r/shogun/shogun-dev/) as:

    sudo docker pull shogun/shogun
    sudo docker run -it shogun/shogun bash

We offer images for both the latest release and nightly development builds.

Sometimes mounting a local folder into the docker image is useful.
You can do this via passing an additional option

```
-v /your/local/folder:/same/folder/in/docker
```

See the Docker documentation for further details.


## Integration with interface language build systems <a name="language"></a>
Shogun is can be automatically built from source from the following langauges.

### Python pypi <a name="pypi"></a>
You can install from [pipy](https://pypi.python.org/pypi/shogun-ml/).
There is limited control over options and it might take a long time as everything is done from scratch.

    pip install shogun-ml

We do not reccomend this option and suggest to rather compile by hand as described below.


# Compiling manually <a name="manual"></a>

In case none of the binary packages listed on our website work for your system, or you want to modify Shogun, you will need to build it from source.

## Requirements <a name="manual-requirements"></a>
The standard GNU/Linux tools and Python are minimal requirements to compile Shogun.
To compile the interfaces, in addition to [swig](http://www.swig.org/) itself, you will need language specific development packages installed, see [interfaces](#manual-interfaces) below.

There is a larger number of optional requirements.
The output of cmake output lists optional dependencies that were found and not found.
If a particular Shogun class is unavailable, this is likely due to an unmet dependency.
See our [docker configuration file](https://github.com/shogun-toolbox/shogun/blob/develop/configs/shogun/Dockerfile) for an example configuration used in our test builds.

You need at least 1GB free disk space. If you compile any interface, roughly 4 GB RAM are need (we are working on reducing this).
[CCache](https://ccache.samba.org/) will massively speed up the compilation process and is enabled by default if installed.

## Basics <a name="manual-basics"></a>
Shogun uses [CMake](https://cmake.org/) for its build. The general workflow is now explained.
For further details on testing etc, see [DEVELOPING.md](DEVELOPING.md).

Download the latest [stable release source code](https://github.com/shogun-toolbox/shogun/releases/latest), or (as demonstrated here) clone the latest develop code.
Potentially update submodules

    git clone https://github.com/shogun-toolbox/shogun.git
    git submodule update --init

Create the build directory in the source tree root

    cd shogun
    mkdir build

Configure cmake, from the build directory, passing the Shogun source root as argument.
It is recommended to use any of CMake GUIs (e.g. replace `cmake ..` with `ccmake ..`), in particular if you feel unsure about possible parameters and configurations.
Note that all cmake options read as `-DOPTION=VALUE`.

    cd build
    cmake [options] ..

Compile

    make


Install (prepend `sudo` if installing system wide), and your are done.

    make install

Sometimes you might need to clean up your build (e.g. in case of some major changes).
First, try

    make clean

If that does not help, try removing the build directory and starting from scratch afterwards

    rm -rf build

If you prefer to not run the `sudo make install` command system wide, you can either install Shogun to a custom location (`-DCMAKE_INSTALL_PREFIX=/custom/path`, defaults to `/usr/local`), or even skip `make install` at all.
In both cases, it is necessary to set a number of system libraries for using Shogun, see [INTERFACES.md](INTERFACES.md).

## Interfaces <a name="manual-interfaces"></a>
The native C++ interface is always included.
The cmake options for building interfaces are `-DPythonModular -DOctaveModular -DRModular -DJavaModular -DRubyModular -DLuaModular -DCSharpModular` etc.
For example, replace the cmake step above by
```
cmake -DPythonModular=ON [potentially more options] ..
```

The required packages (here debian/Ubuntu package names) for each interface are
 * Python
   - `python-dev python-numpy`
 * Octave
   - `octave liboctave-dev`
 * R
   - `r-base-core`
 * Java
   - `oracle-java8-installer`, non-standard, e.g. `https://launchpad.net/~webupd8team/+archive/ubuntu/java`
   - `jblas`, a standard third party library, `https://mikiobraun.github.io/jblas/`
 * Ruby
   - `ruby ruby-dev`, and `narray` a non-standard third party library, `http://masa16.github.io/narray/`, install with `gem install narray`
 * Lua
   - `lua5.1 liblua5.1-0-dev`
 * C-Sharp
   - `mono-devel mono-gmcs cli-common-dev`

To *use* the interfaces, in particular if not installing to the default system-wide location, see [INTERFACES.md](INTERFACES.md).
See [examples](#manual-examples) below for how to create the examples from the website locally.

## Generating examples <a name="manual-examples"></a>
All Shogun examples at our website are automatically generated code. You can
generate them (plus additional ones) locally (needs cmake switch `-DBUILD_META_EXAMPLES=ON`)

    make meta_examples

This requires [PLY for Python](https://pypi.python.org/pypi/ply), package `python-ply`, and [ctags](http://ctags.sourceforge.net/), package `ctags`.
Both source code and potential executables (C++, Java, C-Sharp) are created in `build/examples/meta/` when running `make`.

See [INTERFACES.md](INTERFACES.md) to run the generated examples and see [EXAMPLES.md](EXAMPLES.md) for more details on their mechanics.
See [DEVELOPING.md](DEVELOPING.md) for how the examples are used as tests.

## Problems? Got stuck? Found a bug? Help?  <a name="manual-problems"></a>

In case you have a problem building Shogun, please open an [issue on github](https://github.com/shogun-toolbox/shogun/issues) with your system details, *exact* commands used, and logs posted as a [gist](https://gist.github.com/).
