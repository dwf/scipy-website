Building ATLAS and ATLAS-accelerated LAPACK on Linux
----------------------------------------------------

.. contents::
    :local:


These instructions, contributed by Chris Colbert, should get you started with
the stable release of ATLAS in order to build a complete BLAS and LAPACK
library. For a more complete treatment, see R. Clint Whaley's official
`ATLAS INSTALL Guide <http://math-atlas.sourceforge.net/atlas_install/>`_.

1. Download the current release of 
   `LAPACK <http://www.netlib.org/lapack/lapack-3.2.2.tgz>`_.
2. Download 
   `ATLAS 3.8.3 <http://sourceforge.net/projects/math-atlas/files/Stable/3.8.3/atlas3.8.3.tar.gz/download>`_
   (current stable release at the time of writing, circa July 2010).
3. Create the folder  ``/home/your-user-name/build/atlas``. This is where we
   will build ATLAS.
4. Create the folder ``/home/your-user-name/build/lapack``. This is where we
   will build LAPACK.
5. Extract the folder ``lapack-3.2.2`` to 
   ``/home/your-user-name/build/lapack``.
6. Extract the contents of ``atlas3.8.3.tar.gz`` to ``/home/your-user-name/build/atlas``.
7. Now in the terminal, we'll make sure we have the right Fortran compiler
   and other build tools (Ubuntu-specific).
   
   ::

       sudo apt-get remove g77
       sudo apt-get install gfortran
       sudo apt-get install build-essential

8. Copy the distributed gfortran version of ``make.inc`` to the 
   top-level LAPACK directory:

   ::

       cd /home/your-user-name/build/lapack/lapack-3.2.2
       cp INSTALL/make.inc.gfortran make.inc

9. In the make.inc file make sure the line   
   ::
   
       OPTS = -O2 -fPIC -m64
       NOOPTS = -O0 -fPIC -m64
   
   The ``-m64`` flags build 64-bit code, if you want 32-bit, simply leave
   the ``-m64`` flags out.

10. The following commands should now build LAPACK without incident: 
    ::

        cd SRC
        make

11. We now need to create a directory in which to build our specific
    tuned instance of ATLAS.
    
    ::
    
        cd /home/your-user-name/build/atlas/ATLAS
        mkdir Linux_X64SSE2
        cd Linux_X64SSE2

12. In order for ATLAS to get correct timing information we need to turn
    off all CPU frequency scaling. This is accomplished with
    
    ::

        sudo cpufreq-selector -g performance

    If you have multiple cores, you will need to turn it off for each one. 
    They are numbered from 0, e.g. for two cores,

    ::
    
        sudo cpufreq-selector -g performance -c 0
        sudo cpufreq-selector -g performance -c 1

13. Finally we'll build ATLAS. If you don't want 64-bit libraries, remove
    the ``-b`` 64 flag. Replace the number 2400 with your CPU frequency in MHz.
    
    ::
    
        ../configure -b 64 -D c -DPentiumCPS=2400 -Fa  -alg -fPIC \
            --with-netlib-lapack=/home/your-user-name/build/lapack/lapack-3.2.1/Lapack_LINUX.a

    The configure step takes a while, and should end without errors.

14. This next part takes a long time, go get some coffee. It should end 
    without errors.

    ::
    
        make build

15. This will verify the build, it also takes a while.

    ::
    
        make check

16. This will test the performance of your build and give you feedback on
    it. Your numbers should be close to the test numbers at the end.

    ::
    
        make time

17. Next, we will build shared libraries.

    ::

        cd lib
        make shared    # single-threaded .so's
        make ptshared  # multithreaded .so's


18. Finally, we'll copy all of the ATLAS libraries (and the LAPACK library
    built with ATLAS) to our ``/usr/local/lib``:

    ::
    
        sudo  cp  *.so  /usr/local/lib

You now have a working ATLAS-accelerated multithreaded BLAS and LAPACK
in your ``/usr/local/lib``. When you build NumPy, make sure the 
``site.cfg`` file in your top-level



::

    [DEFAULT]
    library_dirs = /usr/local/lib
    include_dirs = /usr/local/include

    [blas_opt]
    libraries = ptf77blas, ptcblas, atlas

    [lapack_opt]
    libraries = lapack, ptf77blas, ptcblas, atlas
