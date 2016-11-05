### Features
   * GSoC 2016 project of Saurabh Mahindre: Major efficiency improvements for KMeans, LARS, Random Forests, Bagging, KNN.
   * Add new Shogun cookbook for documentation and testing across all target languages [Heiko Strathmann, Sergey Lisitsyn, Esben Sorig, Viktor Gal].
   * Added option to learn CombinedKernel weights with GP approximate inference [Wu Lin].
   * LARS now supports 32, 64, and 128 bit floating point numbers [Chris Goldsworthy].

### Bugfixes:
   * Fix gTest segfaults with GCC >= 6.0.0 [Björn Esser].
   * Make Java and CSharp install-dir configurable [Björn Esser].
   * Autogenerate modshogun.rb with correct module-suffix [Björn Esser].
   * Fix KMeans++ initialization [Saurabh Mahindre].

### Cleanup, efficiency updates, and API Changes:
   * Make Eigen3 a hard requirement. Bundle if not found on system. [Heiko Strathmann]
   * Drop ALGLIB (GPL) dependency in CStatistics and ship CDFLIB (public domain) instead [Heiko Strathmann]
   * Drop p-value estimation in model-selection [Heiko Strathmann]
   * Static interfaces have been removed [Viktor Gal]
   * New base class ShiftInvariantKernel of which GaussianKernel inherits [Rahul De].

### NOTE
This version contains a new CMake option USE_GPL_SHOGUN, which  when set to OFF will exclude all GPL codes from Shogun [Heiko Strathmann].


