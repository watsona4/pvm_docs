:func:`pvm_joingroup`
=====================

.. function:: int pvm_joingroup(char* group)

   The routine :func:`pvm_joingroup` enrolls the calling task in the
   group named `group` and returns the instance number `inum` of this
   task in this group. If there is an error `inum` will be negative.

   Instance numbers start at ``0`` and count up. When using groups a
   (`group`, `inum`) pair uniquely identifies a PVM process. This is
   consistent with the PVM 2.4 naming schemes. If a task leaves a
   group by calling :func:`pvm_lvgroup` and later rejoins the same
   group, the task is not guaranteed to get the same instance
   number. PVM attempts to reuse old instance numbers, so when a task
   joins a group it will get the lowest available instance number. A
   task can be a member of multiple groups simultaneously.

   :param char* group: Character string group name of an existing
      group.
   :returns: Integer instance number returned by the routine.
      Instance numbers start at ``0`` and count up.  Values less than
      zero indicate an error.

Fortran Call
------------

.. f:subroutine:: pvmfjoingroup(group, inum)

   :param character group: Character string group name of an existing
      group.
   :param integer inum: Integer instance number returned by the
      routine.  Instance numbers start at ``0`` and count up.  Values
      less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   inum = pvm_joingroup("worker");

Fortran:

.. code-block:: fortran

   CALL PVMFJOINGROUP('group2', INUM)

Errors
------

These error conditions can be returned by :func:`pvm_joingroup`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` was not started or has crashed.

:macro:`PvmBadParam`
   giving a ``NULL`` group name.

:macro:`PvmDupGroup`
   trying to join a group you are already in.

See Also
--------

:func:`pvm_lvgroup`
