Developer Zone
==============

We need your help!
------------------

This is a distributed, volunteer project with many contributors. The
best way to join our effort is to start using the code and join 
the discussions taking place on the :doc:`mailing-lists` (primarily
"NumPy-discussion" for NumPy and "SciPy-dev" for SciPy).

Source Code
-----------

Make contributions (e.g. code patches), feature requests and file bug reports 
by submitting a "ticket" on the Trac pages linked below.  Because of spam 
abuse, you must create an account on our Trac in order to submit a ticket, 
then click on the "New Ticket" tab that only appears when you have logged in.
Please give as much information as you can in the ticket.  Also specify the
component, the version you are referring to and the milestone.  Report bugs
to the appropriate Trac instance (there is one for NumPy and a different one
for SciPy).  There are read-only mailing lists for tracking the status of 
your bug ticket.


+-------+-------------------------+------------------------------------------+
| NumPy | Developer's Wiki (Trac) | http://projects.scipy.org/numpy          |
|       +-------------------------+------------------------------------------+
|       | Subversion              | http://svn.scipy.org/svn/numpy/trunk     |
|       +-------------------------+------------------------------------------+
|       | Timeline                | http://projects.scipy.org/numpy/timeline |
+-------+-------------------------+------------------------------------------+
| NumPy | Developer's Wiki (Trac) | http://projects.scipy.org/scipy          |
|       +-------------------------+------------------------------------------+
|       | Subversion              | http://svn.scipy.org/svn/scipy/trunk     |
|       +-------------------------+------------------------------------------+
|       | Timeline                | http://projects.scipy.org/scipy/timeline |
+-------+-------------------------+------------------------------------------+



Interested users can get repository write access as well.  This usually 
requires a developer "vouching" for you, which happens more easily if you 
already made a number of patch contributions.

See :ref:`packaging`, below, for the process of building and making releases.

New Code
########

If you have some new code you'd like to see included in SciPy, the first 
thing to do is make a **SciKit**.

A SciKit is a stand-alone package including your code (including complete
reference and user documentation). SciKits are distributed at
http://scikits.appspot.com/. This site is currently still in early construction
phases, and will be moved (and linked) to the main page of this site when it is
ready to accept new SciKits. Right now users and developers are encouraged to
view the site and comment on scipy-dev@scipy.org to ensure it serves your
needs.

Once you get some use experience, the community may decide to include your
SciKit in SciPy. These decisions are based on many factors, including maturity
of the code API and the docs, ease of building it on all platforms, how many
people use it, how well it is integrated into SciPy, etc.

.. _packaging:

Making Source and Binary Releases
---------------------------------

See the `Making Releases <http://projects.scipy.org/numpy/wiki/MakingReleases>`_
page on the developer wiki.


Steering Committee
------------------

In general, decisions about the projects are made by consensus among 
the relevant developers and users through discussions on the mailing
lists. In very rare situations where such an effort proves difficult,
the steering committee is responsible for helping resolve questions
about the direction of the projects.

The steering committee currently consists of

* Jarrod Millman
* Eric Jones
* Robert Kern
* Travis Oliphant
* Stefan van der Walt

It should be emphasized that the committee is "hands off", and that
the vast majority of decisions are made by consensus via 
mailing list discussion, not the steering committee.

