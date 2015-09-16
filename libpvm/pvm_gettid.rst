:func:`pvm_gettid`
==================

.. function:: int pvm_gettid(char* group, int inum)

   The routine :func:`pvm_gettid` returns the tid of the PVM process
   identified by the group name `group` and the instance number
   `inum`. If :func:`pvm_gettid` is successful, `tid` will be ``> 0``.
   If some error occurs then `tid` will be ``< 0``.

   :param char* group: Character string that contains the name of an
      existing group.
   :param int inum: Integer instance number of the process in the
      group.
   :returns: Integer task identifier returned.
   :rtype: int

Fortran Call
------------

.. f:function:: pvmfgettid(group, inum, tid)

   :param character group: Character string that contains the name of
      an existing group.
   :param integer inum: Integer instance number of the process in the
      group.
   :param integer tid: Integer task identifier returned.

Examples
--------

C:

.. code-block:: c

   tid = pvm_gettid("worker", 0);

Fortran:

.. code-block:: fortran

   CALL PVMFGETTID('worker', 5, TID)

Errors
------

These error conditions can be returned by :func:`pvm_gettid`:

:macro:`PvmSysErr`
   Can not contact the local :ref:`pvmd <pvmd3>`; most likely it is
   not running.

:macro:`PvmBadParam`
   Bad Parameter most likely a ``NULL`` character string.

:macro:`PvmNoGroup`
   No group exists by that name.

:macro:`PvmNoInst`
   No such instance in the group.

See Also
--------

:func:`pvm_joingroup`, :func:`pvm_getinst`
