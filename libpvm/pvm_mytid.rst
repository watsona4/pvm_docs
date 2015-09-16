:func:`pvm_mytid`
=================

.. function:: int pvm_mytid(void)

   The routine :func:`pvm_mytid` enrolls this process into PVM on its
   first call. It also generates a unique `tid` if this process was
   not created by :func:`pvm_spawn`. :func:`pvm_mytid` returns the
   `tid` of the calling process and can be called multiple times in an
   application.

   Any PVM system call (not just :func:`pvm_mytid`) will enroll a task
   in PVM if the task is not enrolled before the call.

   The `tid` is a 32 bit positive integer created by the local
   :ref:`pvmd <pvmd3>`. The 32 bits are divided into fields that
   encode various information about this process such as its location
   in the virtual machine (i.e., local :ref:`pvmd <pvmd3>` address),
   the CPU number in the case where the process is on a
   multiprocessor, and a process ID field. This information is used by
   PVM and is not expected to be used by applications. Applications
   should not attempt to predict or interpret the `tid` with the
   exception of calling :func:`pvm_tidtohost`.

   If PVM has not been started before an application calls
   :func:`pvm_mytid` the returned `tid` will be ``< 0``.

   :returns: Integer returning the task identifier of the calling PVM
      process. Values less than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfmytid(tid)

   :param integer tid: Integer returning the task identifier of the
      calling PVM process. Values less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   tid = pvm_mytid();

Fortran:

.. code-block:: fortran

   CALL PVMFMYTID(TID)

Errors
------

This error condition can be returned by :func:`pvm_mytid`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding.

See Also
--------

:func:`pvm_tidtohost`, :func:`pvm_parent`
