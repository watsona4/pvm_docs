:func:`pvm_catchout`
====================

.. function:: int pvm_catchout(FILE* ff)

   :param FILE* ff: File descriptor on which to write collected
      output.
   :returns: Integer status code returned by the routine. Values less
      than zero indicate an error.
   :rtype: int

   The routine :func:`pvm_catchout` causes the calling task (the
   parent) to catch output from any tasks spawned after the call to
   :func:`pvm_catchout`.  Characters printed on :file:`stdout` or
   :file:`stderr` in children tasks are collected by the :ref:`pvmd
   <pvmd3>`\ s and sent in control messages to the parent task, which
   tags each line and appends it to the specified file. Output from
   grandchildren (spawned by children) tasks is also collected,
   provided the children don't reset their :macro:`PvmOutputTid`.

   If option :macro:`PvmShowTids` (see :func:`pvm_setopt`) is true
   (nonzero), output is printed as shown below, tagged with the task
   ID where the output originated::

     [txxxxx] BEGIN [txxxxx] (text from child task) [txxxxx] END

   The output from each task includes one ``BEGIN`` line and one
   ``END`` line, with whatever the task prints in between. If
   :macro:`PvmShowTids` is false, raw output is printed with no
   additional information.

   In C, the output file descriptor may be specified. Giving a null
   pointer turns output collection off for any subsequently spawned
   child tasks. (Any existing output collection will still proceed at
   the child tasks, until they exit or change their
   :macro:`PvmOutputTid` or related settings---see page for
   :func:`pvm_setopt`.)

   If :func:`pvm_exit` is called while output collection is in effect,
   it will block in order to print all the output, until all tasks
   sending the given task output have exited. To avoid this, output
   collection can be turned off by calling :func:`pvm_catchout(0)
   <pvm_catchout>` before calling :func:`pvm_exit`.

   :func:`pvm_catchout` always returns ``0``.

Fortran Call
------------

.. f:subroutine:: pvmfcatchout(onoff, info)

   :param integer onoff: Integer parameter. Turns output collection on
      or off.
   :param integer info: Integer status code returned by the
      routine. Values less than zero indicate an error.

   In Fortran, output collection can only be turned on or off (again
   only for subsequently spawned child tasks), and is always logged to
   the :file:`stdout` of the parent task.

Examples
--------

C:

.. code-block:: c

   #include <stdio.h>

   pvm_catchout(stdout);

Fortran:

.. code-block:: fortran

   CALL PVMFCATCHOUT(1, INFO)

See Also
--------

:func:`pvm_exit`, :func:`pvm_setopt`, :func:`pvm_spawn`
