:func:`pvm_lvgroup`
===================

.. function:: int pvm_lvgroup(char* group)

   The routine :func:`pvm_lvgroup` unenrolls the calling process from
   the group named `group`. If there is an error `info` will be
   negative.

   If a process leaves a group by calling either :func:`pvm_lvgroup`
   or :func:`pvm_exit`, and later rejoins the same group, the process
   may be assigned a new instance number. Old instance numbers are
   reassigned to processes calling :func:`pvm_joingroup`.

   :param char* group: Character string group name of an existing
      group.
   :returns: Integer status code returned by the routine. Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmflvgroup(group, info)

   :param character group: Character string group name of an existing
      group.
   :param integer info: Integer status code returned by the
      routine. Values less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   info = pvm_lvgroup("worker");

Fortran:

.. code-block:: fortran

   CALL PVMFLVGROUP('group2', INFO)

Errors
------

These error conditions can be returned by :func:`pvm_lvgroup`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding.

:macro:`PvmBadParam`
   giving a ``NULL`` group name.

:macro:`PvmNoGroup`
   giving a non-existent group name.

:macro:`PvmNotInGroup`
   asking to leave a group you are not a member of.

See Also
--------

:func:`pvm_joingroup`
