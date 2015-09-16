:func:`pvm_bufinfo`
===================

.. function:: int pvm_bufinfo(int bufid, int* bytes, int* msgtag, int* tid)

   The routine :func:`pvm_bufinfo` returns information about the
   requested message buffer. Typically it is used to determine facts
   about the last received message such as its size or source.
   :func:`pvm_bufinfo` is especially useful when an application is
   able to receive any incoming message, and the action taken depends
   on the source `tid` and the `msgtag` associated with the message
   that comes in first. If :func:`pvm_bufinfo` is successful, `info`
   will be ``0``. If some error occurs then `info` will be ``< 0``.

   :param int bufid: Integer specifying a particular message buffer
      identifier.
   :param int* bytes: Integer returning the length in bytes of the
      body of the message. This will be equal to the actual size of
      the data packed, if :macro:`PvmDataRaw` is used, otherwise it
      may include pad bytes.
   :param int* msgtag: Integer returning the message label. Useful
      when the message was received with a wildcard `msgtag`.
   :param int* tid: Integer returning the source of the message.
      Useful when the message was received with a wildcard `tid`.
   :returns: Integer status code returned by the routine. Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfbufinfo(bufid, bytes, msgtag, tid, info)

Examples
--------

C:

.. code-block:: c

   bufid = pvm_recv(-1, -1);
   info = pvm_bufinfo(bufid, &bytes, &type, &source);

Fortran:

.. code-block:: fortran

   CALL PVMFRECV(-1, -1, BUFID)
   CALL PVMFBUFINFO(BUFID, BYTES, TYPE, SOURCE, INFO)

Errors
------

These error conditions can be returned by :func:`pvm_bufinfo`:

:macro:`PvmNoSuchBuf`
   specified buffer does not exist.

:macro:`PvmBadParam`
   invalid argument

See Also
--------

:func:`pvm_recv`
