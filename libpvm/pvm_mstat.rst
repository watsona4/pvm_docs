:func:`pvm_mstat`
=================

.. function:: int pvm_mstat(char* host)

   The routine :func:`pvm_mstat` returns the status `mstat` of the
   computer named `host` with respect to running PVM processes. This
   routine can be used to determine if a particular host has failed
   and if the virtual machine needs to be reconfigured. The function
   :func:`pvm_notify` can also be used to notify the caller that a
   host has failed.

   The return status values are:

   ==================== ==============================================
   Value                Meaning
   ==================== ==============================================
   :macro:`PvmOk`       host is OK
   :macro:`PvmNoHost`   host is not in virtual machine
   :macro:`PvmHostFail` host is unreachable (and thus possibly failed)
   ==================== ==============================================

   :param char* host: Character string containing the host name.
   :returns: Integer return status code.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfmstat(host, mstat)

   :param character host: Character string containing the host name.
   :param integer mstat: Integer machine status code.

Examples
--------

C:

.. code-block:: c

   mstat = pvm_mstat("msr.ornl.gov");

Fortran:

.. code-block:: fortran

   CALL PVMFMSTAT('msr.ornl.gov', MSTAT)

Errors
------

These error conditions can be returned by :func:`pvm_mstat`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding.

:macro:`PvmNoHost`
   giving a host name not in the virtual machine.

:macro:`PvmHostFail`
   host is unreachable (and thus possibly failed).

See Also
--------

:func:`pvm_notify`, :func:`pvm_config`
