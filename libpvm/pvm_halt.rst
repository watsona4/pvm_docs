:func:`pvm_halt`
================

.. function:: int pvm_halt(void)

   The routine :func:`pvm_halt` shuts down the entire PVM system
   including remote tasks, remote :ref:`pvmd <pvmd3>`\ s, the local
   tasks (including the calling task) and the local :ref:`pvmd
   <pvmd3>`.

   The task calling :func:`pvm_halt` must ignore or catch signal
   :macro:`SIGTERM` or it will be killed along with all the others.

   :returns: Integer returns the error status.
   :rtype: int

Fortran Call
------------

.. f:function:: pvmfhalt(info)

   :param integer info: Integer returns the error status.

Errors
------

The following error conditions can be returned:

:macro:`PvmSysErr`
   local :ref:`pvmd <pvmd3>` is not responding.

See Also
--------

:func:`pvm_delhosts`, :func:`pvm_kill`, :func:`pvm_exit`
