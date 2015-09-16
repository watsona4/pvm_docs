:func:`pvm_barrier`
===================

.. function:: int pvm_barrier(char* group, int count)

   The routine :func:`pvm_barrier` blocks the calling process until
   count members of the `group` have called :func:`pvm_barrier`. The
   `count` argument is required because processes could be joining the
   given group after other processes have called
   :func:`pvm_barrier`. Thus PVM doesn't know how many group members
   to wait for at any given instant. Although count can be set less,
   it is typically the total number of members of the group. So the
   logical function of the :func:`pvm_barrier` call is to provide a
   group synchronization. During any given barrier call all
   participating group members must call barrier with the same count
   value. Once a given barrier has been successfully passed,
   :func:`pvm_barrier` can be called again by the same group using the
   same group name.

   If :func:`pvm_barrier` is successful, `info` will be ``0``. If some
   error occurs then `info` will be ``< 0``.

   :param char* group: Character string group name. The group must
      exist and the calling process must be a member of the group.
   :param int count: Integer specifying the number of group members
      that must call :func:`pvm_barrier` before they are all
      released. Though not required, `count` is expected to be the
      total number of members of the specified group.
   :returns: Integer status code returned by the routine. Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfbarrier(group, count, info)

Examples
--------

C:

.. code-block:: c

   inum = pvm_joingroup("worker");
     .
     .
   info = pvm_barrier("worker", 5);

Fortran:

.. code-block:: fortran

   CALL PVMFJOINGROUP("shakers", INUM)
   COUNT = 10
   CALL PVMFBARRIER("shakers", COUNT, INFO)

Errors
------

These error conditions can be returned by :func:`pvm_barrier`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` was not started or has crashed.

:macro:`PvmBadParam`
       giving a count < 1.

:macro:`PvmNoGroup`
       giving a non-existent group name.

:macro:`PvmNotInGroup`
       calling process is not in specified group.

See Also
--------

:func:`pvm_joingroup`
