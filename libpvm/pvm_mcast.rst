:func:`pvm_mcast`
=================

.. function:: int pvm_mcast(int* tids, int ntask, int msgtag)

   The routine :func:`pvm_mcast` multicasts a message stored in the
   active send buffer to `ntask` tasks specified in the `tids`
   array. The message is not sent to the caller even if listed in the
   array of `tids`. The content of the message can be distinguished by
   `msgtag`. If :func:`pvm_mcast` is successful, `info` will be
   ``0``. If some error occurs then `info` will be ``< 0``.

   The receiving processes can call either :func:`pvm_recv` or
   :func:`pvm_nrecv` to receive their copy of the multicast.
   :func:`pvm_mcast` is asynchronous and based on a minimum spanning
   tree algorithm between the :ref:`pvmd <pvmd3>`\ s. Computation on
   the sending processor resumes as soon as the message is safely on
   its way to the receiving processors. This is in contrast to
   synchronous communication, during which computation on the sending
   processor halts until the matching receive is executed by the
   receiving processor.

   :func:`pvm_mcast` first determines which other :ref:`pvmd <pvmd3>`\
   s contain the specified tasks. Then passes the message to these
   :ref:`pvmd <pvmd3>`\ s which in turn distribute the message to
   their local tasks without further network traffic.

   Multicasting is not supported by most multiprocessor vendors.
   Typically their native calls only support broadcasting to all the
   user's processes on a multiprocessor. Because of this omission,
   :func:`pvm_mcast` may not be an efficient communication method on
   some multiprocessors except in the special case of broadcasting to
   all PVM processes.

   :param int* tids: Integer array of length `ntask` containing the
      task IDs of the tasks to be sent to.
   :param int ntask: Integer specifying the number of tasks to be sent
      to.
   :param int msgtag: Integer message tag supplied by the user.
      `msgtag` should be ``>= 0``.  It allows the user's program to
      distinguish between different kinds of messages .
   :returns: Integer status code returned by the routine. Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfmcast(ntask, tids, msgtag, info)

   :param integer ntask: Integer specifying the number of tasks to be
      sent to.
   :param integer tids: Integer array of length `ntask` containing
      the task IDs of the tasks to be sent to.
   :param integer msgtag: Integer message tag supplied by the user.
      `msgtag` should be ``>= 0``.  It allows the user's program to
      distinguish between different kinds of messages .
   :param integer info: Integer status code returned by the
      routine. Values less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   info = pvm_initsend(PvmDataRaw);
   info = pvm_pkint(array, 10, 1);
   msgtag = 5;
   info = pvm_mcast(tids, ntask, msgtag);

Fortran:

.. code-block:: fortran

   CALL PVMFINITSEND(PVMDEFAULT)
   CALL PVMFPACK(REAL4, DATA, 100, 1, INFO)
   CALL PVMFMCAST(NPROC, TIDS, 5, INFO)

Errors
------

These error conditions can be returned by :func:`pvm_mcast`:

:macro:`PvmBadParam`
   giving a `msgtag` ``< 0``.

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding.

:macro:`PvmNoBuf`
   no send buffer.

See Also
--------

:func:`pvm_psend`, :func:`pvm_recv`, :func:`pvm_send`
