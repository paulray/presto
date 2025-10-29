## Version 5.2.0:
 * Now require GSL (Gnu Scientific Library) to be installed. This library is thread-safe and allows us to more easily parallelize routines.
 * Large set of changes that updated K&R-style declarations so that they would compile with GCC v15. These changes were made by Claude Code(!) with the prompting by Paul Ray. Thanks, Paul and Claude!

## Version 5.1.0:
 * Updated ATNF Pulsar Catalog to v2.65
 * Three new and useful python programs / utilities:
   * `stacksearch.py` Read multiple PRESTO-style `*.fft` files and conduct a stack search for periodicities.
   * `fourier_fold.py` Use the complex amplitudes in a PRESTO `.fft` file (or in multiple FFT files) to generate pulse profiles without having to do time-domain folding.
   * `pfdzap.py` Perform simple time- and/or frequency domain zapping of `.pfd` files. Generate zap commands for `show_pfd`, `get_TOAs.py`, and `sum_profiles.py`.
 * Many small bug fixes and tweaks, including more correct handling of DM smearing in `DDplan.py`

## Version 5.0.3:
 * Updated ATNF Pulsar Catalog to v2.51
 * Added an experimental version of `fit_circular_orbit` using sliders in `examplescripts`
 * Added the ability to use `pygaussfit.py` without middle or right mouse buttons
 * Fixed a couple memory issues in `rednoise`, thanks to @bwmeyers
 * Explicitly set the random number seeds (for reproducibility) in `makedata`
 * Several other very minor tweaks and bug fixes

## Version 5.0.2:
 * Updated the C wrappers for PGPLOT for the Numpy 2.0 C API 
 * Python v3.9 or newer is now required.
 * Several minor bug fixes, including to `injectpsr.py`, thanks to @remsforian

## Version 5.0.1:
 * Minor improvements over v5.0.0
 * Some clarifications and improvements to the build process
 * Addition of new recipes for docker / singularity images (thanks Alessandro Ridolfi!)
 * Bugfix in `rednoise` having to do with absolute paths (thanks Alessandro Ridolfi!)
 * Bugfix that checks to see if the maskfile you intend to use matches the properties of the data you are using

## Version 5.0.0:
 * This is a major release since I've moved to a completely different and modern build system: [meson](https://mesonbuild.com/), along with the [meson-python](https://meson-python.readthedocs.io/en/latest/) backend. This was required since *Numpy* has deprecated `numpy.distutils` and this caused python builds to stop working with Python v3.12.
   * See the [INSTALL.md]()https://github.com/scottransom/presto/blob/master/INSTALL.md) for updated installation instructions.
   * You will need to install **meson**, **meson-python**, and **ninja**, but that is easily done via `pip`!
   * Python v3.8 or newer is now required.
 * All of the old Spigot-related codes have been removed. If you need to process Spigot data, please use the `classic` branch that is mentioned in the README.md.
 * All of the `slalib` codes (and the python interface to it) have been removed. If you need that stuff, you should transition to [ERFA](https://github.com/liberfa/erfa) and/or [Astropy](https://www.astropy.org/).
 * There are two nice new python utilities:
   * `binary_utils.py` reads a parfile of a binary pulsar and computes min/max observed barycentric spin periods or velocities as either a function of the orbit (default), or for a prescribed duration of time, and optionally plots those. It also shows basic information about the binary.
   * `compare_periods.py` compares candidate spin periods and their integer and fractional harmonics with one or more parfiles to a prescribed fractional tolerance. 

## Version 4.0:
 * This is a major release since it involves big changes to the Python portions of the codebase:
   * Python v3.7 or newer is now required.
   * A long-standing memory issue was fixed with Anaconda Python (running `python tests/test_presto_python.py` will tell you if you have that issue or not).
   * Swig v4 is used to generate the Python wrappers of the PRESTO C library.
   * Big thanks to **Shami Chatterjee** and **Bradley Meyers** who helped me get to the bottom of this!
 * There is a [FAQ](https://github.com/scottransom/presto/blob/master/FAQ.md) with lots of information!
 * PRESTO has a dockerfile that allows it to build on Docker Hub automatically. Thanks to **Nick Swainston** for this!  (more testing and improvements would be welcome)
 * `simple_zapbirds.py` makes it much easier to manually zap interference from simple searches (no need for copying ".inf" files and running both `makezaplist.py` and `zapbirds`).
 * `realfft` and `zapbirds` can now be called on many files at once on the command line. This benefits HPC systems which often don't like many programs running serially on many small files.
 * A new python interface to the internal `prepfold` folding code (`simplefold`), as well as wrappers of fast `C` implementations of $\chi^2$ and $Z^2_N$ (thanks to **Matteo Bachetti**).
 * Many bug fixes and minor improvements, including one that would cause segfaults with very large dispersion sweeps in `prepdata` and `prepsubband`, and a problem with `prepfold` significance calculations.

## Version 3.0.1:
 * This is a minor release which fixes several issues and adds some minor improvements:
   * Fix of long-standing `rfifind` bug that could cause the program to hang if channels had zero variance
   * Multiple Python3-related bug fixes
   * Added `-debug` flag to `prepfold` to allow debugging of TEMPO calls to make polycos
   * `DDplan.py` can now read observation parameters from filterbank or PSRFITS input files. And you can write a `dedisp_*.py` dedispersion script, based on the plan, using the `-w` option
   * The `rednoise` program now writes a corresponding *_red.inf file
   * Update of the Tutorial document, including a new slide on red noise

## Version 3.0:
 * This major release of PRESTO includes a massive restructuring of python code and capabilities. Things should work with Python versions 2.7 and Python 3.6 and 3.7 at least. The installation of the python code has changed and has become more "pythonic" so that `PYTHONPATH` is not needed, and all of the various modules are now under a top-level "presto" module. For example, to use the psr_utils module you would now do:
   
   `import presto.psr_utils as pu`
   
   rather than

   `import psr_utils as pu`

   All of these changes will likely lead to code breakage and bugs!

   Please check your code and processing carefully and post issues (and hopefully pull requests) if you find them.

   The installation instructions have been updated in the INSTALL file.

   Huge thanks thanks go to **Gijs Molenaar, Matteo Bachetti, and Paul Ray** for the work that they have done helping with this!

 * There is also a new `examplescripts` directory where you will find some example code to do a lot of important things, like
   * Fully dedispersing an observation: `dedisp.py`
   * Fully searching a dedispersed observation: `full_analysis.py`
   * Sifting the results of a full search: `ACCEL_sift.py`
   * Searching short chunks of a long time series: `short_analysis_simple.py`
   * Making a really nice P-Pdot plane: `ppdot_plane_plot.py`
   * and a few others.

## Version 2.2:
 * Version 2.2 was the last version of PRESTO to work with the old-style python interface which requires Python v2.7 or earlier and is "installed" in-place and used via having `$PRESTO/lib/python` in your `PYTHONPATH`. There will probably be occasional bug fixes for v2.2 in the `v2.2maint` branch of PRESTO. You can get it using: `git checkout -b v2.2maint origin/v2.2maint`, and then installing as per the INSTALL file.

## Version 2.1:
 * `accelsearch` now has a "jerk" search capability (thanks to (then) UVA undergrad **Bridget Andersen** for help with this!). This makes searches take a *lot* longer, but definitely improves sensitivity when the observation duration is 5-15% of the duration of the orbital period.  Typically `-wmax` should be set to 3-5x `-zmax` (and you probably never need to set `-zmax` to anything larger than 300).
 * Ability to ignore bad channels on the command line (-ignorechan) (see `rfifind_stats.py` and `weights_to_ignorechan.py`)
