:func:`pvm_nrecv`
=================

.. function:: int pvm_nrecv(int tid, int msgtag)

   The routine :func:`pvm_nrecv` checks to see if a message with label
   `msgtag` has arrived from `tid`, and also clears the current
   receive buffer if any. If a matching message has arrived
   :func:`pvm_nrecv` immediately places the message in a new active
   receive buffer, and returns the buffer identifier in `bufid`.

   If the requested message has not arrived, then :func:`pvm_nrecv`
   immediately returns with a ``0`` in `bufid`. If some error occurs
   `bufid` will be ``< 0``.

   A ``-1`` in `msgtag` or `tid` matches anything. This allows the
   user the following options. If ``tid = -1`` and `msgtag` is defined
   by the user, then :func:`pvm_nrecv` will accept a message from any
   process which has a matching msgtag. If ``msgtag = -1`` and `tid`
   is defined by the user, then :func:`pvm_nrecv` will accept any
   message that is sent from process `tid`. If ``tid = -1`` and
   ``msgtag = -1``, then :func:`pvm_nrecv` will accept any message
   from any process.

   The PVM model guarantees the following about message order. If task
   1 sends message **A** to task 2, then task 1 sends message **B** to
   task 2, message **A** will arrive at task 2 before message **B**.
   Moreover, if both messages arrive before task 2 does a receive,
   then a wildcard receive will always return message **A**.

   :func:`pvm_nrecv` is non-blocking in the sense that the routine
   always returns immediately either with the message or with the
   information that the message has not arrived at the local
   :ref:`pvmd <pvmd3>` yet. :func:`pvm_nrecv` can be called multiple
   times to check if a given message has arrived yet. In addition the
   blocking receive :func:`pvm_recv` can be called for the same
   message if the application runs out of work it could do before the
   data arrives.

   If :func:`pvm_nrecv` returns with the message, then the data in the
   message can be unpacked into the user's memory using the unpack
   routines.

   :param int tid: Integer task identifier of sending process supplied
      by the user.
   :param int msgtag: Integer message tag supplied by the user.
      msgtag should be ``>= 0``.
   :returns: Integer returning the value of the new active receive
      buffer identifier. Values less than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfnrecv(tid, msgtag, bufid)

   :param integer tid: Integer task identifier of sending process
      supplied by the user.
   :param integer msgtag: Integer message tag supplied by the user.
      msgtag should be ``>= 0``.
   :param integer bufid: Integer returning the value of the new active
      receive buffer identifier. Values less than zero indicate an
      error.

Examples
--------

C:

.. code-block:: c

   tid = pvm_parent();
   msgtag = 4;
   arrived = pvm_nrecv(tid, msgtag);
   if (arrived > 0)
     info = pvm_upkint(tid_array, 10, 1);
   else
     /* go do other computing */

Fortran:

.. code-block:: fortran

   CALL PVMFNRECV(-1, 4, ARRIVED)
   IF (ARRIVED.gt.0) THEN
     CALL PVMFUNPACK(INTEGER4, TIDS, 25, 1, INFO)
     CALL PVMFUNPACK(REAL8, MATRIX, 100, 100, INFO)
   ELSE
     ! GO DO USEFUL WORK
   ENDIF

Errors
------

These error conditions can be returned by :func:`pvm_nrecv`:

:macro:`PvmBadParam`
   giving an invalid `tid` value or `msgtag`.

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding.

See Also
--------

:func:`pvm_bufinfo`, :func:`pvm_getminfo`, :func:`pvm_recv`,
:func:`pvm_unpack`, :func:`pvm_send`, :func:`pvm_mcast`
