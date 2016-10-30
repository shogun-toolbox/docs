* SHOGUN Release version 5.0.0 (libshogun 17.2, data 0.11, parameter 1)

* NOTE: This version contains a new CMake option USE_GPL_SHOGUN, which
        when set to OFF will exclude all GPL codes from Shogun [Heiko Strathmann].
* Features:
    - Add new Shogun cookbook for documentation and testing across all 
      target languages [Heiko Strathmann, Sergey Lisitsyn, Esben Sorig, Viktor Gal].
* Bugfixes:
	- Fix gTest segfaults with GCC >= 6.0.0 [Björn Esser].
	- Make Java and CSharp install-dir configurable [Björn Esser].
	- Autogenerate modshogun.rb with correct module-suffix [Björn Esser].
* Cleanup, efficiency updates, and API Changes:
    - Make Eigen3 a hard requirement. Bundle if not found on system. [Heiko Strathmann]
    - Drop ALGLIB (GPL) dependency in CStatistics and ship CDFLIB (public domain) instead [Heiko Strathmann]
    - Drop p-value estimation in model-selection [Heiko Strathmann]
    - Static interfaces have been removed [Viktor Gal]
