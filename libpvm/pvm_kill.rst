:func:`pvm_kill`
================

.. function:: int pvm_kill(int tid)

   The routine :func:`pvm_kill` sends a terminate (:macro:`SIGTERM`)
   signal to the PVM process identified by `tid`. In the case of
   multiprocessors the terminate signal is replaced with a host
   dependent method for killing a process. If :func:`pvm_kill` is
   successful, `info` will be ``0``. If some error occurs then `info`
   will be ``< 0``.

   :func:`pvm_kill` is not designed to kill the calling process. To
   kill yourself in C call :func:`pvm_exit` followed by :func:`exit`.
   To kill yourself in Fortran call :f:func:`pvmfexit` followed by
   ``stop``.

   :param int tid: Integer task identifier of the PVM process to be
      killed (not yourself).
   :returns: Integer status code returned by the routine.  Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfkill(tid, info)

   :param integer tid: Integer task identifier of the PVM process to
      be killed (not yourself).
   :param integer info: Integer status code returned by the routine.
      Values less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   info = pvm_kill(tid);

Fortran:

.. code-block:: fortran

   CALL PVMFKILL(TID, INFO)

Errors
------

These error conditions can be returned by :func:`pvm_kill`:

:macro:`PvmBadParam`
   giving an invalid `tid` value.

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding.

See Also
--------

:func:`pvm_exit`, :func:`pvm_halt`
