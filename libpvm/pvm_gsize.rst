:func:`pvm_gsize`
=================

.. function:: int pvm_gsize(char* group)

   The routine :func:`pvm_gsize` returns the size of the group named
   `group`. If there is an error size will be negative.

   Since groups can change dynamically in PVM 3.0, this routine can
   only guarantee to return the instantaneous size of a given group.

   :param char* group: Character string group name of an existing
      group.
   :returns: Integer returning the number of members presently in the
      group. Values less than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:function:: pvmfgsize(group, size)

   :param character group: Character string group name of an existing
      group.
   :param integer size: Integer returning the number of members
      presently in the group. Values less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   size = pvm_gsize("worker");

Fortran:

.. code-block:: fortran

   CALL PVMFGSIZE('group2', SIZE)

Errors
------

These error conditions can be returned by :func:`pvm_gsize`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` was not started or has crashed.

:macro:`PvmBadParam`
   giving an invalid group name.

See Also
--------

:func:`pvm_joingroup`
