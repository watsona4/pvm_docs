:func:`pvm_exit`
================

.. function:: int pvm_exit(void)

   The routine :func:`pvm_exit` tells the local :ref:`pvmd <pvmd3>`
   that this process is leaving PVM. This routine does not kill the
   process, which can continue to perform tasks just like any other
   serial process.

   :func:`pvm_exit` should be called by all PVM processes before they
   stop or exit for good. It must be called by processes that were
   not started with :func:`pvm_spawn`.

   :returns: Integer status code returned by the routine. Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfexit( info )

   :param integer info: Integer status code returned by the routine.
      Values less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   /* Program done */
   pvm_exit();
   exit();

Fortran:

.. code-block:: fortran

   CALL PVMFEXIT(INFO)
   STOP

Errors
------

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding
