.. _pvm-shmd:

PVM Shared Memory Daemon
========================

.. program:: pvm_shmd

:ref:`Pvm_shmd <pvm-shmd>` is a daemon process which maintains and
handles the usage of shared memory resources (memory segments,
semaphores, message queues) on a particular PVM host. One
:ref:`pvm_shmd <pvm-shmd>` must run on each host in a virtual machine
that wants to use the ``shmd`` shared memory message passing
layer. The ``shmd`` message passing layer allows local processes to a
host to use shared memory for message passing on that host *only*.
See the ``shmd`` directory :file:`README` for further notes and
restrictions.

The daemon can be started manually as with any other PVM task. Only
one :ref:`pvm_shmd <pvm-shmd>` can be active per host. If the
:ref:`pvm_shmd <pvm-shmd>` detects another :ref:`pvm_shmd <pvm-shmd>`
it will shutdown.

The :ref:`pvm_shmd <pvm-shmd>` can be killed from the PVM console by
using the :option:`pvm reset` command. Local and remote :ref:`pvm_shmd
<pvm-shmd>`\ s can also be started from the PVM console program
:ref:`pvm <pvm>`.

The name of the daemon executable is :ref:`pvm_shmd <pvm-shmd>`.

Shared Resource Usage
^^^^^^^^^^^^^^^^^^^^^

The :ref:`pvm_shmd <pvm-shmd>` will attempt to create upto
:macro:`MAXSEG` shared memory segments of up to :macro:`MAXPAGES`
memory pages in size. The size of each memory page is architecture
dependent (see :func:`getpagesize`). The segments are then numbered
``0...N-1`` where ``N`` is the number of segments created. The first
segement (``0``) has the shared memory control structure placed in its
head. This structure allows alien processes to located the other
shared memory segments and any required controlling information.

The message passing layer allocates memory from these segments
asynchronously without any interaction with the :ref:`pvm_shmd
<pvm-shmd>` using semaphores to protect data during updates to any
associated structures. For each segment there is an associated page
map of which processes have currently locked a page. Each page map for
a segment has a separate semaphore protecting it. The semaphores are
accessed with the :macro:`SEM_UNDO` flag set so that if a process
holding a semaphore should die, the OS (should) reset the semaphore
automatically, thus allowing any waiting/blocked processes to
continue.

The :ref:`pvm_shmd <pvm-shmd>` only maintains the segments and their
allocation page map(s). Thus if a process allocated pages in a
segment and then exits, it is the :ref:`pvm_shmd <pvm-shmd>` that
detects this and then frees the allocated pages.

The :ref:`pvm_shmd <pvm-shmd>` can have its status checked at any time
by using the :program:`pvm_shmd_stat` process to kick it into
reporting onto either ``stdio`` or the pvm log file (:file:`pvml.uid`)
its internal state.

The shared memory and all associated processes can be cleared by
sending the :ref:`pvm_shmd <pvm-shmd>` a ``HUP`` signal.

Sending the :ref:`pvm_shmd <pvm-shmd>` a ``TERM`` signal will just
cause it to clear any shared resources and then exit.

The :ref:`pvm_shmd <pvm-shmd>` cannot catch the ``KILL`` signal.

If the :ref:`pvm_shmd <pvm-shmd>` is killed without clearing all of
its shared resources these can be cleared by calling
:program:`ipcfree` which resides in the :file:`pvm3/lib` directory.

Message Passing using SHMD
^^^^^^^^^^^^^^^^^^^^^^^^^^

The :ref:`pvm_shmd <pvm-shmd>` handles resources that are used by
special versions of :func:`pvm_psend` and :func:`pvm_precv` stored in
the :file:`libpvmshmd.a` library. Thus to use these facilities,
applications have to link to this library instead of the usual
:file:`libpvm3.a` library.

Options
-------

The following options may be specified on the command line when
starting the :ref:`pvm_shmd <pvm-shmd>`:

.. option:: -debug=level

   Sets the :ref:`pvm_shmd <pvm-shmd>` debug level. Used to debug the
   :ref:`pvm_shmd <pvm-shmd>` or :file:`libpvmshmd` (not intended to
   be used to debug application programs).

.. option:: -maxsegs=maxsegs

   Sets the maximum number of segments that the :ref:`pvm_shmd
   <pvm-shmd>` can create. This is used to over-ride the compiled in
   value from :file:`shmd.h`. Note that the value cannot be above the
   :macro:`MAXSEGS` in the :file:`shmd.h` file.

.. option:: -maxpages=maxpages

   Sets the maximum segment size to maxpages pages of memory. This
   value cannot be above the compiled value :macro:`MAXPAGES` in
   :file:`shmd.h` or the actual OS defined limit.

Notes
-----

Remember that :ref:`pvm_shmd <pvm-shmd>` allocated memory from the VM
available on the machine. Allocating more segements improves
performance as there is less sharing of segments (semaphores for their
page maps). Although you must remember to leave some memory available
for normal program and OS system usage, as the :ref:`pvm_shmd
<pvm-shmd>` allocated memory is *only* used for message passing.

Files
-----

* :file:`$PVM_ROOT/lib/$PVM_ARCH/pvm_shmd`: PVM shared memory daemon
  executable
* :file:`$PVM_ROOT/shmd/shmd.h`: Shared Memory hard limits header file
* :file:`/tmp/pvml.uid`: Pvmd runtime error log

See Also
--------

:ref:`pvm <pvm>`, :ref:`pvmd <pvmd3>`, :func:`getpagesize`,
:program:`ipcs`, :func:`msgctl`, :func:`semctl`, :func:`shmctl`,
:func:`signal`
