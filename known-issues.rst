..
  Keep these known issues updated to the current state of the software.
  
  Maintain the existing headers in Installation Issues and simply report "None"
  if there are no issues at the moment.

############
Known Issues
############

.. _installation-issues:

Binary installation issues
==========================

*No known issues.*

.. _src-installation-issues:

Source installation issues
==========================

.. _installation-issues-cross-platform:

Cross Platform
--------------

- Compiling some packages---in particular ``afw``\ ---require large amounts of
  RAM to compile. This is compounded as the system will automatically attempt
  to parallelize the build, and can cause the build to run extremely slowly or
  fail altogether. On machines with less than 8 GB of RAM, disable
  parallelization by setting ``EUPSPKG_NJOBS=1`` in your environment before
  running ``eups distrib``.

.. _installation-issues-redhat:

Red Hat (and clones) specific
-----------------------------

RHEL 7.*
^^^^^^^^

*No known issues.*

RHEL 6.*
^^^^^^^^

- If you have a problem building on **RHEL 6** check the :ref:`Pre-requisites
  <source-install-redhat-prereqs>` to make sure sure you are using a more
  recent version of :command:`gcc` (minimum required is 4.8)

- curl looks for certificates in :file:`/etc/pki/tls/certs/ca-bundle.crt`
  rather than :file:`/etc/ssl/certs/ca-certificates.crt`. The solution is to
  copy :file:`ca-certificates.crt` to :file:`ca-bundle.crt`.

.. _installation-issues-macos:

macOS specific
--------------

macOS 10.12 (Sierra) and OS X 10.11 (El Capitan)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- `MPICH`_ version 3.2, as currently distributed with the stack, fails
  regularly and unpredictably with a segmentation fault on macOS systems.
  MPICH is used by the `ctrl_pool`_ task distribution framework, and hence the
  `pipe_drivers`_ top-level scripts package which provides the following
  executables:

  - :file:`coaddDriver.py`
  - :file:`constructBias.py`
  - :file:`constructDark.py`
  - :file:`constructFlat.py`
  - :file:`constructFringe.py`
  - :file:`multiBandDriver.py`
  - :file:`singleFrameDriver.py`

  It should be possible to run these commands by restricting them to a single
  CPU core (i.e., ``--batch-type=smp --cores=1``).

  This issue will be resolved by upgrading to version 3.3 of MPICH when it
  becomes available. :jirab:`DM-7588`

.. _MPICH: http://www.mpich.org/
.. _ctrl_pool: https://github.com/lsst/ctrl_pool
.. _pipe_drivers: https://github.com/lsst/pipe_drivers

Older systems
^^^^^^^^^^^^^

- Some old installations of XCode on Macs create a :file:`/Developer`
  directory.  This can interfere with installation.

- Macs must use the :command:`clang` compiler, not :command:`gcc`.
  :jirab:`DM-3405`

  One version of this problem occurs when using Macports_, which, by
  default, will create a symlink from :file:`/opt/local/bin/c++` to its
  version of :command:`g++`. Try removing that, starting a new shell, and
  restarting :command:`eups distrib install`.

.. _Macports: https://www.macports.org/index.php
