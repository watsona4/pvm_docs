:func:`pvm_gather`
==================

.. function:: int pvm_gather(void* result, void* data, int count, int datatype, int msgtag, char* group, int rootginst)

   :func:`pvm_gather` performs a send of messages from each member of
   the `group` to the specified root member of the group. All group
   members must call :func:`pvm_gather`, each sends its array `data`
   of length `count` of `datatype` to the root which accumulates these
   messages into its `result` array. It is as if the root receives
   `count` elements of `datatype` from the ``i``\ th member of the
   `group` and places these values in its `result` array starting with
   offset ``i*count`` from the beginning of the `result` array. The
   root task is identified by its instance number in the group.

   C and Fortran defined datatypes are:

   ================== =================
   C datatypes        FORTRAN datatypes
   ================== =================
   :type:`PVM_BYTE`   :type:`BYTE1`
   :type:`PVM_SHORT`  :type:`INTEGER2`
   :type:`PVM_INT`    :type:`INTEGER4`
   :type:`PVM_FLOAT`  :type:`REAL4`
   :type:`PVM_CPLX`   :type:`COMPLEX8`
   :type:`PVM_DOUBLE` :type:`REAL8`
   :type:`PVM_DCPLX`  :type:`COMPLEX16`
   :type:`PVM_LONG`
   ================== =================

   In using the scatter and gather routines, keep in mind that C
   stores multidimensional arrays in row order, typically starting
   with an initial index of ``0``; whereas, Fortran stores arrays in
   column order, typically starting with an offset of ``1``.

   .. note:: :func:`pvm_gather` does not block. If a task calls
      :func:`pvm_gather` and then leaves the group before the root has
      called :func:`pvm_gather` an error may occur.

   The current algorithm is very simple and robust. A future
   implementation may make more efficient use of the architecture to
   allow greater parallelism.

   :param void* result: On the root this is a pointer to the starting
      address of an array `datatype` of local values which are to be
      accumulated from the members of the `group`. If ``n`` is the
      number of members in the group, then this array of datatype
      should be of length at least ``n*count``. This argument is
      meaningful only on the root.
   :param void* data: For each group member this is a pointer to the
      starting address of an array of length `count` of `datatype`
      which will be sent to the specified root member of the group.
   :param int count: Integer specifying the number of elements of
      datatype to be sent by each member of the group to the root.
   :param int datatype: Integer specifying the type of the entries in
      the result and data arrays. (See above for defined types.)
   :param int msgtag: Integer message tag supplied by the user.
      `msgtag` should be ``>= 0``. It allows the user's program to
      distinguish between different kinds of messages.
   :param char* group: Character string group name of an existing
      group.
   :param int rootginst: Integer instance number of group member who
      performs the gather of the messages from the members of the
      group.
   :returns: Integer status code returned by the routine. Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfgather(result, data, count, datatype, msgtag, group, rootginst, info)

Examples
--------

C:

.. code-block:: c

   info =  pvm_gather(&getmatrix, &myrow, 10, PVM_INT,
		      msgtag, "workers", rootginst);

Fortran:

.. code-block:: fortran

   CALL PVMFGATHER(GETMATRIX, MYCOLUMN, COUNT, INTEGER4, &
                   MTAG, 'workers', ROOT, INFO)

Errors
------

These error conditions can be returned by :func:`pvm_gather`:

:macro:`PvmNoInst`
   Calling task is not in the group

:macro:`PvmBadParam`
   The datatype specified is not appropriate

:macro:`PvmSysErr`
   Pvm system error

See Also
--------

:func:`pvm_bcast`, :func:`pvm_barrier`, :func:`pvm_psend`
