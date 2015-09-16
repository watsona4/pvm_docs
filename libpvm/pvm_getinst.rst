:func:`pvm_getinst`
===================

.. function:: int pvm_getinst(char* group, int tid)

   The routine :func:`pvm_getinst` takes a group name `group` and a
   PVM task identifier `tid` and returns the unique instance number
   that corresponds to the input. It can be called by any task whether
   in the group or not. If :func:`pvm_getinst` is successful, `inum`
   will be ``>= 0``. If some error occurs then `inum` will be ``< 0``.

   :param char* group: Character string group name of an existing
      group.
   :param int tid: Integer task identifier of a PVM process.
   :returns: Integer instance number returned by the routine.
      Instance numbers start at ``0`` and count up. Values less than
      zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfgetinst(group, tid, inum)

   :param character group: Character string group name of an existing
      group.
   :param integer tid: Integer task identifier of a PVM process.
   :param integer inum: Integer instance number returned by the
      routine.  Instance numbers start at ``0`` and count up. Values
      less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   inum = pvm_getinst("worker", pvm_mytid());
   --------
   inum = pvm_getinst("worker", tid[i]);

Fortran:

.. code-block:: fortran

   CALL PVMFGETINST('GROUP3', TID, INUM)

Errors
------

These error conditions can be returned by :func:`pvm_getinst`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` was not started or has crashed.

:macro:`PvmBadParam`
   giving an invalid `tid` value.

:macro:`PvmNoGroup`
   giving a non-existent group name.

:macro:`PvmNotInGroup`
   specifying a `group` in which the `tid` is not a member.

See Also
--------

:func:`pvm_joingroup`, :func:`pvm_gettid`
