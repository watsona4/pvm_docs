:func:`pvm_bcast`
=================

.. function:: int pvm_bcast(char* group, int msgtag)

   .. versionchanged:: 3.2
      The broadcast message is not sent back to the sender. 

   The routine :func:`pvm_bcast` broadcasts a message stored in the
   active send buffer to all the members of `group`. Any PVM task can
   call :func:`pvm_bcast`, it need not be a member of the group.  The
   content of the message can be distinguished by `msgtag`. If
   :func:`pvm_bcast` is successful, `info` will be ``0``. If some
   error occurs then `info` will be ``< 0``.

   :func:`pvm_bcast` is asynchronous. Computation on the sending
   processor resumes as soon as the message is safely on its way to
   the receiving processors. This is in contrast to synchronous
   communication, during which computation on the sending processor
   halts until a matching receive is executed by all the receiving
   processors.

   :func:`pvm_bcast` first determines the ``tids`` of the group
   members by checking a group data base. A multicast is performed to
   these ``tids``. If the group is changed during a broadcast the
   change will not be reflected in the broadcast. Multicasting is not
   supported by most multiprocessor vendors. Typically their native
   calls only support broadcasting to all the user's processes on a
   multiprocessor. Because of this omission, :func:`pvm_bcast` may not
   be an efficient communication method on some multiprocessors.

   :param char* group: Character string group name of an existing group.
   :param int msgtag: Integer message tag supplied by the user.
      `msgtag` should be ``>= 0``. It allows the user's program to
      distinguish between different kinds of messages.
   :returns: Integer status code returned by the routine. Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfbcast(group, msgtag, info)

Examples
--------

C:

.. code-block:: c

   info = pvm_initsend(PvmDataRaw);
   info = pvm_pkint(array, 10, 1);
   msgtag = 5;
   info = pvm_bcast("worker", msgtag);

Fortran:

.. code-block:: fortran

   CALL PVMFINITSEND(PVMDEFAULT)
   CALL PVMFPKFLOAT(DATA, 100, 1, INFO)
   CALL PVMFBCAST('worker', 5, INFO)

Errors
------

These error conditions can be returned by :func:`pvm_bcast`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` was not started or has crashed.

:macro:`PvmBadParam`
   giving a negative `msgtag`.

:macro:`PvmNoGroup`
   giving a non-existent group name.

See Also
--------

:func:`pvm_joingroup`
