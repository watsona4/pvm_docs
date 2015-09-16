:func:`pvm_notify`
==================

.. function:: int pvm_notify(int what, int msgtag, int cnt, int* tids)

   The routine :func:`pvm_notify` requests PVM to notify the caller on
   detecting certain events. One or more notify messages (see below)
   are sent by PVM back to the calling task. The messages have tag
   `msgtag` supplied to notify.

   The notification messages have the following format:

   .. macro:: PvmTaskExit

      One notify message for each TID requested. The message body
      contains a single TID of exited task.

   .. macro:: PvmHostDelete

      One notify message for each TID requested. The message body
      contains a single :ref:`pvmd <pvmd3>`\ -TID of exited :ref:`pvmd
      <pvmd3>`.

   .. macro:: PvmHostAdd

      `cnt` notify messages are sent, one each time the local
      :ref:`pvmd <pvmd3>`\ 's host table is updated. The message body
      contains an integer length followed by a list of :ref:`pvmd
      <pvmd3>`\ -TIDs of new :ref:`pvmd <pvmd3>`\ s. The counter of
      :macro:`PvmHostAdd` messages yet to be sent is replaced by
      successive calls to :func:`pvm_notify`. Specifying a `cnt` of
      ``-1`` turns on :macro:`PvmHostAdd` messages until a future
      notify; a count of zero disables them.

   TIDs in the notify messages are packed as integers.

   The calling task is responsible for receiving messages with the
   specified tag and taking appropriate action.

   Future versions of PVM may expand the list of available
   notification events.

   :param int what: Type of event to trigger the notification.
      Presently one of: :macro:`PvmTaskExit`, :macro:`PvmHostDelete`,
      or :macro:`PvmHostAdd`.
   :param int msgtag: Message tag to be used in notification.
   :param int cnt: For :macro:`PvmTaskExit` and
      :macro:`PvmHostDelete`, specifies the length of the `tids` array.
      For :macro:`PvmHostAdd`, specifies the number of times to notify.
   :param int* tids: For :macro:`PvmTaskExit` and
      :macro:`PvmHostDelete`, an array of length `cnt` of task or
      :ref:`pvmd <pvmd3>` TIDs to be notified about. The array is not
      used with the :macro:`PvmHostAdd` option.
   :returns: Integer status code returned by the routine. Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfnotify(what, msgtag, cnt, tids, info)

   :param integer what: Type of event to trigger the notification.
      Presently one of: :macro:`PvmTaskExit`, :macro:`PvmHostDelete`,
      or :macro:`PvmHostAdd`.
   :param integer msgtag: Message tag to be used in notification.
   :param integer cnt: For :macro:`PvmTaskExit` and
      :macro:`PvmHostDelete`, specifies the length of the `tids` array.
      For :macro:`PvmHostAdd`, specifies the number of times to notify.
   :param integer tids: For :macro:`PvmTaskExit` and
      :macro:`PvmHostDelete`, an array of length `cnt` of task or
      :ref:`pvmd <pvmd3>` TIDs to be notified about. The array is not
      used with the :macro:`PvmHostAdd` option.
   :param integer info: Integer status code returned by the
      routine. Values less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   info = pvm_notify(PvmTaskExit, 9999, ntask, tids);

Fortran:

.. code-block:: fortran

   CALL PVMFNOTIFY(PVMHOSTDELETE, 1111, NUMHOSTS, DTIDS, INFO)

To "cancel" a notify request in PVM, the :func:`pvm_notify` routine
can be reinvoked with an additional :macro:`PvmNotifyCancel` flag in
the what argument. The remaining arguments to this cancelling
invocation must match the original invocation exactly, aside from the
additional :macro:`PvmNotifyCancel` which can be added(``+``) or
OR-ed(``|``) to the what argument:

.. code-block:: c

   pvm_notify(PvmTaskExit, 9999, ntask, tids);

   . . .

   pvm_notify(PvmTaskExit | PvmNotifyCancel, 9999, ntask, tids);

Note that when a notify is cancelled, the notify message is delivered,
as if the given event (i.e., task exit, host add or delete) had
occurred.

Errors
------

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding.

:macro:`PvmBadParam`
   giving an invalid argument value.

See Also
--------

:func:`pvm_tasks`, :func:`pvm_config`
