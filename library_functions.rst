.. _libpvm:

#####################
PVM Library Functions
#####################

.. toctree::
   :hidden:
   :glob:

   pvm/pvm_*

All PVM applications must be linked with the :ref:`libpvm <libpvm>`
library to allow them to communicate with other entities in the PVM
system. The base library (:file:`libpvm3.a`) is written in C and
directly supports C and C++ applications. The Fortran library
(:file:`libfpvm3.a`) consists of wrapper functions to convert Fortran
calling sequences to C.

Applications written in C must be linked with at least the base PVM
library, :file:`libpvm3.a`. Fortran applications must be linked with
both :file:`libfpvm3.a` and :file:`libpvm3.a`. On some operating
systems, PVM programs must be linked with other vendor-provided
libraries (containing for example, socket or XDR functions).

Programs that use group functions must also be linked with
:file:`libgpvm3.a`.

Subroutines
===========

The :ref:`libpvm <libpvm>` subroutines can be divided into roughly
five classes:

Message Passing
---------------

==================== =
:func:`pvm_addmhf`   Install message-handler functions
:func:`pvm_bufinfo`  Returns information about a message buffer
:func:`pvm_freebuf`  Disposes of a message buffer
:func:`pvm_delmhf`   Remove message-handler functions
:func:`pvm_getrbuf`  Returns the message buffer identifier for the active receive buffer
:func:`pvm_getsbuf`  Returns the message buffer identifier for the active send buffer
:func:`pvm_initsend` Clear default send buffer and specify message encoding
:func:`pvm_mcast`    Multicasts the data in the active message buffer to a set of tasks
:func:`pvm_mkbuf`    Creates a new message buffer
:func:`pvm_nrecv`    Non-blocking receive
:func:`pvm_packf`    Pack the active message buffer with arrays of prescribed data type
:func:`pvm_precv`
:func:`pvm_probe`
:func:`pvm_psend`
:func:`pvm_recv`
:func:`pvm_recvf`
:func:`pvm_send`
:func:`pvm_sendsig`
:func:`pvm_setmwid`
:func:`pvm_setrbuf`
:func:`pvm_setsbuf`
:func:`pvm_trecv`
:func:`pvm_unpack`
==================== =

Task Control
------------

================== =
:func:`pvm_exit`   Tells the local :ref:`pvmd <pvmd3>` that this process is leaving PVM
:func:`pvm_kill`   Terminates a specified PVM process
:func:`pvm_mytid`  Returns the tid of the calling process
:func:`pvm_parent`
:func:`pvm_pstat`
:func:`pvm_spawn`
:func:`pvm_tasks`
================== =

Group Library Functions
-----------------------

======================= =
:func:`pvm_barrier`     Blocks the calling process until all processes in a group have called it
:func:`pvm_bcast`       Broadcasts the data in the active message buffer to a group of processes
:func:`pvm_freezegroup` Freezes  dynamic  group  membership and caches info locally
:func:`pvm_gather`      A  specified  member of the group receives messages from each member of the group and  gathers  these  messages  into  a  single array
:func:`pvm_getinst`     Returns the instance number in a group of a PVM process
:func:`pvm_gettid`      Returns the tid of the process identified by a group name and instance number
:func:`pvm_gsize`       Returns the number of members presently in the named group
:func:`pvm_joingroup`   Enrolls the calling process in a named group
:func:`pvm_lvgroup`     Unenrolls the calling process from a named group
:func:`pvm_reduce`
:func:`pvm_scatter`
======================= =

Virtual Machine Control
-----------------------

====================== =
:func:`pvm_addhosts`   Add hosts to the virtual machine
:func:`pvm_config`     Returns information about the present virtual machine configuration
:func:`pvm_delhosts`   Deletes hosts from the virtual machine
:func:`pvm_getfds`     Get file descriptors in use by PVM
:func:`pvm_halt`       Shuts down the entire PVM system
:func:`pvm_mstat`      Returns the status of a host in the virtual machine
:func:`pvm_reg_hoster`
:func:`pvm_reg_rm`
:func:`pvm_reg_tasker`
:func:`pvm_start_pvmd`
====================== =

Miscellaneous
-------------

======================= =
:func:`pvm_advise`      Controls use of direct task-to-task routing
:func:`pvm_archcode`    Returns the data representation code for a PVM architecture name
:func:`pvm_catchout`    Catch output from child tasks
:func:`pvm_delete`      Delete data from :ref:`pvmd <pvmd3>` database
:func:`pvm_delinfo`     Deletes an entry from the database
:func:`pvm_export`      Mark environment variables to export through spawn
:func:`pvm_freecontext` Free existing context
:func:`pvm_getcontext`  Get current context
:func:`pvm_getopt`      Returns the value of :ref:`libpvm <libpvm>` options
:func:`pvm_hostsync`    Get time-of-day clock from PVM host
:func:`pvm_insert`      Store data in :ref:`pvmd <pvmd3>` database
:func:`pvm_newcontext`  Request new context
:func:`pvm_notify`      Request notification of PVM event such as host failure
:func:`pvm_perror`
:func:`pvm_setcontext`  Change context
:func:`pvm_setopt`
:func:`pvm_settmask`
:func:`pvm_tidtohost`
:func:`pvm_unexport`    Mark environment variables to unexport
======================= =

Deprecated Functions
--------------------

The following functions are deprecated and should not be used.

===================== =
:func:`pvm_advise`    Controls use of direct task-to-task routing
:func:`pvm_delete`    Delete data from :ref:`pvmd <pvmd3>` database
:func:`pvm_insert`    Store data in :ref:`pvmd <pvmd3>` database
:func:`pvm_lookup`    Retrieve data from :ref:`pvmd <pvmd3>` database
===================== =

Errors
======

On success, most :ref:`libpvm <libpvm>` functions return the constant
PvmOk.  The following error conditions can be returned by :ref:`libpvm
<libpvm>` functions:

.. macro:: PvmAlready

   The requested operation requires exclusive access, and another was
   already in progress.

.. macro:: PvmBadMsg

   The received messages has a data format native to another machine,
   which cannot be decoded by :ref:`libpvm <libpvm>`.

.. macro:: PvmBadParam

   A bad parameter was passed to the function.

.. macro:: PvmBadVersion

   Two PVM components (:ref:`pvmd <pvmd3>` and task, two :ref:`pvmd
   <pvmd3>`\ s or two tasks) have incompatible protocol versions and
   cannot interoperate. Version mismatch.

.. macro:: PvmCantStart

   A :ref:`pvmd <pvmd3>` could not be started on the local host, or a
   slave :ref:`pvmd <pvmd3>` could not be started on a remote host.

.. macro:: PvmDSysErr

   Some internal mechanism in the :ref:`pvmd <pvmd3>` failed during
   the requested operation.

.. macro:: PvmDupEntry

   The class server already has an entry matching the insert request.

   .. deprecated:: 3.4
      Relaced by :macro:`PvmExists`

.. macro:: PvmDupGroup

   The task has already a member of the group it attempted to join.

.. macro:: PvmDupHost

   An attempt was made to add the same host to a virtual machine more
   than once, or to add a host already a member of another virtual
   machine owned by the same user.

.. macro:: PvmDenied

   Operation is refused due to locking, permissions, etc.

.. macro:: PvmExists

   There is already an entry matching the insert request.

.. macro:: PvmHostFail

   A foreign host in the virtual machine failed during the requested
   operation.

.. macro:: PvmMismatch

   A parameter does not match a corresponding one.

.. macro:: PvmNoBuf

   There is no current message buffer to pack or unpack.

.. macro:: PvmNoData

   The end of a message buffer was reached while trying to unpack
   data.

.. macro:: PvmNoEntry

   The class server has no entry matching the lookup request.

   .. deprecated:: 3.4
      Relaced by :macro:`PvmNotFound`

.. macro:: PvmNoFile

   The named executable does not exist.

.. macro:: PvmNoGroup

   The named group does not exist.

.. macro:: PvmNoHost

   There is no host in the virtual machine with the given name, or the
   name could not be resolved to an address.

.. macro:: PvmNoInst

   The named group has no member with this instance.

.. macro:: PvmNoMem

   Malloc failed to get memory for :ref:`libpvm <libpvm>`.

.. macro:: PvmNoParent

   This task has no parent task.

.. macro:: PvmNoSuchBuf

   There is no message buffer with the given buffer handle.

.. macro:: PvmNoTask

   No task exists with the given tid.

.. macro:: PvmNotFound

   No entry matching the lookup request was found.

.. macro:: PvmNotImpl

   This :ref:`libpvm <libpvm>` function or option is not implemented.

.. macro:: PvmNotInGroup

   The named group has no such member task.

.. macro:: PvmNullGroup

   A null group name was passed to a function.

.. macro:: PvmOutOfRes

   The requested operation could not be completed due to lack of
   resources.

.. macro:: PvmOverflow

   A value is too large to be packed or unpacked.

.. macro:: PvmSysErr

   :ref:`Libpvm <libpvm>` could not contact a :ref:`pvmd <pvmd3>` on
   the local host, or the :ref:`pvmd <pvmd3>` failed during an
   operation.

Files
-----

* :file:`$PVM_ROOT/include/fpvm3.h`: Fortran header file
* :file:`$PVM_ROOT/include/pvm3.h`: C header file
* :file:`$PVM_ROOT/include/pvmsdpro.h`: Header file for tasker, hoster
  and resource manager tasks
* :file:`$PVM_ROOT/include/pvmtev.h`: Header file for tasks
  manipulating trace events
* :file:`$PVM_ROOT/lib/$PVM_ARCH/libpvm3.a`: C (base) library
* :file:`$PVM_ROOT/lib/$PVM_ARCH/libfpvm3.a`: Fortran wrapper library
* :file:`$PVM_ROOT/lib/$PVM_ARCH/libgpvm3.a`: Group function library

See Also
--------

:ref:`aimk <aimk>`, :ref:`pvm <pvm>`, :ref:`pvm-intro`, :ref:`pvmd3
<pvmd3>`
