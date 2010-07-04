===========================================
Building NumPy & SciPy From Source on Linux
===========================================

Introduction: source and binaries
---------------------------------

* On Linux, SciPy and NumPy official releases are source-code
  only. Installing NumPy and SciPy from source is reasonably easy;
  However, both packages depend on other software, some of them
  which can be challenging to install, or shipped with
  incompatibilities by major Linux distributions. Hopefully, you can
  install NumPy and SciPy without any software outside the necessary
  tools to build Python extensions, as most dependencies are
  optional.

* Most major Linux distributions now ship NumPy and SciPy and,
  although the situation is still far from optimal, those binary
  packages are now reasonably reliable to use. Many other binary
  options are also available, ranging from individually made packages
  by some SciPy developers for a specific Linux version, to whole
  commercially-supported scientific distributions. However, keep in
  mind that if you want to use the last improvements done to NumPy
  and SciPy on Linux, you have to build it from sources. It is often
  convenient to use `virtualenv <http://pypi.python.org/pypi/virtualenv>`_
  to isolate different versions of libraries you install from source.

You will find below some installation instructions and advice for
most major distributions.

Building ATLAS
--------------

ATLAS is a BLAS/LAPACK implementation which tunes itself on a specific
machine to provide the best performance, and often match vendor specific
implementations. Unfortunately, building ATLAS is not easy.  But it
is getting easier all the time, and if you plan to use NumPy for a lot
of linear algebra computation, it can be worth it. See :doc:`linux-atlas-lapack`
for some contributed instructions on building these libraries. If you
aren't terribly concerned with speed and more concerned with ease of use
and prevention of headaches, ATLAS (with LAPACK bundled) packages are often
available for several basic CPU configurations. Ubuntu and Debian provide
``libatlas3gf``, ``libatlas3gf-sse``, ``libatlas3gf-sse2``, etc.

Install required packages
-------------------------

To build NumPy, you'll need various compilers, libraries and development
headers. On Ubuntu, you should be able to get the necessities with
the command:

::

    sudo apt-get install build-essential python-dev


For SciPy you'll need a Fortran compiler. We recommend **gfortran**.
It's often a good idea to remove the older g77 if it's installed
as they are not ABI-compatible and accidentally building some things
with g77 can cause problems.

::

    sudo apt-get remove g77
    sudo apt-get install gfortran

To run the test suites, you'll need to install **nose**. We recommend
doing so directly from PyPI i.e. rather than your distribution's
``python-nose`` package or equivalent, since it's good to have the
latest version. On Ubuntu/Debian, this is accomplished with

::

    sudo apt-get remove python-nose
    sudo apt-get install python-setuptools
    sudo easy_install nose



Intel C compiler and MKL
------------------------

The Intel C compiler and Intel Math Kernel Library are free for
personal non-commercial use (note that this does not include
academic use).

Add some variation of the following lines to ``site.cfg`` in your top
level NumPy directory to use MKL:

::

    [mkl]
    library_dirs = /home/youruser/intel/mkl/8.1/lib/32
    mkl_libs = mkl, vml
    include_dirs = /home/youruser/intel/mkl/8.1/include


There are also libraries for the IA-64 and EM64T processors.

Modify ``cc_exe`` in ``numpy/numpy/distutils/intelccompiler.py`` to be
something like:

::

    cc_exe = 'icc -O2 -g -fomit-frame-pointer -mcpu=pentium4 -mtune=pentium4 -march=pentium4 -msse2 -axWN -Wall'


Run ``icc --help`` for more information on processor-specific options.

Compile and install NumPy with the Intel compiler:

::

    python setup.py config --compiler=intel build_clib --compiler=intel build_ext --compiler=intel install


Compile and install SciPy with the Intel compilers:

::

    python setup.py config --compiler=intel --fcompiler=intel build_clib \
        --compiler=intel --fcompiler=intel build_ext --compiler=intel \
        --fcompiler=intel install


You'll have to set ``LD_LIBRARY_PATH`` to

::

    ~/intel/mkl/8.1/lib/32/:~/intel/cc/9.1.044/lib

(exact values will depend on your architecture, compiler and library
versions) for NumPy to work. This can still cause problems. The only
solution I've found that always works is to build Python, NumPy and
SciPy inside an environment where you've set the ``LD_RUN_PATH``
variable, e.g:

::

    export LD_RUN_PATH=~/opt/lib:~/intel/cc/9.1.044/lib:~/intel/fc/9.1.039/lib:~/intel/mkl/8.1/lib/32

Configure Python with ``--prefix=$HOME/opt``, make, make install,
add ``$HOME/opt/bin`` to the front of your PATH and then build
NumPy and SciPy with the ``site.cfg`` as above in their top level
directories (check the config step's output carefully to make sure it
selects MKL). Built like this, you shouldn't have to set any
``LD_LIBRARY_PATH`` for NumPy and SciPy to work. Run the test
suites to verify this.

Distribution-specific information
---------------------------------

Debian "sarge"
##############

If you install NumPy or SciPy ontop of a debian sarge installation
for a CPU with SSE2, there is a bug in libc6 2.3.2 affecting floating
point operations (fixed in version 2.3.3). Due to this bug, the NumPy
and SciPy tests crach with a SIGFPE. Since there is now patch
available, in order to fix this the libc6 sources need to be
downloaded, fixed, and rebuilt. See `Andrew Straw's instructions`_
for more information.

.. _Andrew Straw's instructions: http://www.its.caltech.edu/~astraw/coding.html#libc-patched-for-debian-sarge-to-fix-floating-point-exceptions-on-sse2

