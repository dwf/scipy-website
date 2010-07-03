Building NumPy and SciPy on Mac OS X
------------------------------------

General Notes
=============

Building NumPy and SciPy on Mac OS X requires the `Xcode
developer tools <http://developer.apple.com/technologies/xcode.html>`_,
available as a free download from Apple.

Note that Apple ships their own custom version of Python located in
``/System/Library/Frameworks/Python.framework``
and certain versions (i.e. 10.5, 10.6) ship with outdated versions of
NumPy. **It is recommended not to use the System Python** since an
installed copy of NumPy would overwrite files installed by the system,
which is usually not a good idea (however, one can install into an
alternate prefix or use a virtual environment to avoid such conflicts;
see :ref:`using-syspython-osx`).

The *simplest* solution is to use the Python installer from
`python.org <http://www.python.org/download/>`_, which installs
under ``/Library/Frameworks/Python.framework``. Though the Python.org
distributed binaries are "universal binaries" for PowerPC or Intel Macs,
they are currently **32-bit only**, which may limit your ability
to work with larger datasets and memory mapped files. If you want
to use Python and NumPy in 64-bit mode, it is recommended to use
one of the :ref:`software-dist` or build a 64-bit version of
Python itself from source. The tarball distribution of Python
available at Python.org contains a script to build capable of building
32-bit, 64-bit or mixed-architecture disk image installers. For example,
from the decompressed tarball directory, typing

::

    cd Mac/BuildScript && python build-installer.py \
        --sdk-path=/Developer/SDKs/MacOSX10.5.sdk \
        --dep-target=10.5 --universal-archs=x86_64

will build a disk image installer for a 64-bit Intel build
of Python. Replacing ``x86_64`` with ``64-bit`` will build a
dual-architecture binary for 64-bit PowerPC and 64-bit Intel; using ``all``
will build a quad-architecture ``ppc``, ``i386``, ``x86_64`` and ``ppc64``
disk image installer, with both ``python-32`` and ``python-64`` executables.
The filename of the resulting disk image (usually somewhere in ``/tmp``)
will be printed upon completion.

On Mac OS X 10.6, the System Python is 64-bit.

You can check what version of Python is accessible at the command
line using the command

::

    which python

If you've installed another Python but the output of the above
starts with ``/usr/bin``, you will need to modify your ``PATH``
environment variable to point to the one you've installed.

Building NumPy on Mac OS X
==========================

Once you've settled on which Python you're going to use, simply typing

::

    python setup.py build
    sudo python setup.py install

from the decompressed source directory
should be sufficient to build and install NumPy. This will link
against the Apple-shipped ``Accelerate.framework`` BLAS implementation.
It is also possible to build against potentially faster BLAS
implementations such as ATLAS or the Intel Math Kernel
Library by editing ``site.cfg`` in the top level source directory,
but the built-in BLAS is sufficient for most purposes.

Building SciPy on Mac OS X
==========================

SciPy requires NumPy to be installed first. You will also require a
Fortran compiler. The recommended compiler is the **gfortran** distribution
available from `AT&T Research <http://r.research.att.com/tools/>`_.

Once you've installed NumPy and gfortran, installation once again
consists merely of changing to the decompressed source code directory
and

::

    python setup.py build
    sudo python setup.py install

You may see some warnings about libraries that can't be located. These
are typically optional dependencies and can safely be ignored.

.. _using-syspython-osx:

Using the System Python Safely
==============================

It may be preferable in some situations to install NumPy and SciPy using
the System Python. In order to do this safely, two options are available.
In both cases, the ``sudo`` before install commands is typically unnecessary
if you have write permissions on the chosen directory.

Using ``--prefix``
##################

The ``python setup.py install`` command can take an alternate prefix for the
installation
by specifying ``--prefix=<some_directory>`` to your ``python setup.py install``
command , e.g. ``--prefix=$HOME`` will install to
``lib/python2.6/site-packages`` in your home directory. To use libraries
installed in this way requires you to have the directory in your
``PYTHONPATH`` environment variable.

Using virtualenv
################

The `virtualenv <http://pypi.python.org/pypi/virtualenv>`_ package creates
a virtual installation of Python in an alternate location using symbolic
links. Creating and installing NumPy and SciPy within a created virtualenv
will leave the existing system ``site-packages`` directory untouched. See
the virtualenv documentation for more details.
