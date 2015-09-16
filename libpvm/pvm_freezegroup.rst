:func:`pvm_freezegroup`
=======================

.. function:: int pvm_freezegroup(char* group , int size)

   The routine :func:`pvm_freezegroup` makes a dynamic group named
   `group` static. The group information is then "cached" by all group
   members. :func:`pvm_freezegroup` is a synchronizing routine and
   must be called by all group members to complete. `size` indicates
   the size the dynamic group should be when made static. A value of
   ``-1`` indicates that the current size of the group should be used.
   info returns error information.

   Once a dynamic group has been frozen with :func:`pvm_freezegroup`,
   all subsequent operations that can be satisfied with local data use
   the locally held information. For processes that are outside of the
   group, the first group call, e.g., :func:`pvm_bcast`, will cause
   the static group information to be copied to the calling process.
   Subsequent operations then use the local information. Barriers are
   still arbitrated by the group server.

   Group members should call :func:`pvm_lvgroup` to leave the group
   and free any allocated structures that hold the group information.
   Processes not in the group may call :func:`pvm_lvgroup` to free any
   locally allocated structures. In this case, an error code of
   :macro:`PvmNotInGroup` or :macro:`PvmNoGroup` will be returned to
   the caller.

   Barrier are always arbitrated by the group server, even if the
   group has been made static with :func:`pvm_freezegroup`. If a
   process leaves a static group while other process are waiting at a
   barrier, then :macro:`PvmNoGroup` is returned to all processes
   waiting at the barrier. Future barrier calls with the defunct
   static group, return the same error.

   :param char* group: Character string group name of an existing
      group.
   :param int size: Size of the group when it is frozen.
   :returns: The size of `group` on success. Values less than 0
      indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmffreezegroup(group, size, info)

   :param character group: Character string group name of an existing
      group.
   :param integer size: Size of the group when it is frozen.
   :param integer info: The size of `group` on success. Values less
      than 0 indicate an error.

Examples
--------

C:

.. code-block:: c

   inum = pvm_joingroup("worker");
   info = pvm_freezegroup( "worker", size );

Fortran:

.. code-block:: fortran

   CALL PVMFJOINGROUP('group2', inum)
   CALL PVMFFREEZEGROUP( 'group2', size, info )

Errors
------

These error conditions can be returned by :func:`pvm_freezegroup`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` was not started or has crashed.

:macro:`PvmBadParam`
   giving a ``NULL`` group name.

:macro:`PvmDupGroup`
   trying to freeze a group that is already frozen.

:macro:`PvmNotInGroup`
   trying to freeze a group that you are not in.

Bugs
----

* There is no way to unfreeze a group.
* Processes are not notified if a frozen group becomes invalid.
* Having a non-member process call :func:`pvm_lvgroup` to free
  structures is a bit strange.

See Also
--------

:func:`pvm_barrier`, :func:`pvm_lvgroup`
