:func:`pvm_freebuf`
===================

.. function:: int pvm_freebuf(int bufid)

   The routine :func:`pvm_freebuf` frees the memory associated with
   the message buffer identified by `bufid`.  Message buffers are
   created by :func:`pvm_mkbuf`, :func:`pvm_initsend`, and
   :func:`pvm_recv`.  If :func:`pvm_freebuf` is successful, `info`
   will be ``0``. If some error occurs then info will be ``< 0``.

   :func:`pvm_freebuf` can be called for a send buffer created by
   :func:`pvm_mkbuf` after the message has been sent and is no longer
   needed.

   Receive buffers typically do not have to be freed unless they have
   been saved in the course of using multiple buffers. But
   :func:`pvm_freebuf` can be used to destroy receive buffers as well.
   So messages that arrive but are no longer needed can be destroyed
   so they will not consume buffer space.

   Typically multiple send and receive buffers are not needed and the
   user can simply use the :func:`pvm_initsend` routine to reset the
   default send buffer.

   There are several cases where multiple buffers are useful. One
   example where multiple message buffers are needed involves
   libraries or graphical interfaces that use PVM and interact with a
   running PVM application but do not want to interfere with the
   application's own communication.

   When multiple buffers are used they generally are made and freed
   for each message that is packed. In fact, :func:`pvm_initsend`
   simply does a :func:`pvm_freebuf` followed by a :func:`pvm_mkbuf`
   for the default buffer.

   :param int bufid: Integer message buffer identifier.
   :returns: Integer status code returned by the routine.  Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmffreebuf(bufid, info)

   :param integer bufid: Integer message buffer identifier.
   :param integer info: Integer status code returned by the routine.
      Values less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   bufid = pvm_mkbuf(PvmDataDefault);
     :
   info = pvm_freebuf(bufid);

Fortran:

.. code-block:: fortran

   CALL PVMFMKBUF(PVMDEFAULT, BUFID)
     :
   CALL PVMFFREEBUF(BUFID, INFO)

Errors
------

These error conditions can be returned by :func:`pvm_freebuf`:

:macro:`PvmBadParam`
   giving an invalid argument value.

:macro:`PvmNoSuchBuf`
   giving an invalid bufid value.

See Also
--------

:func:`pvm_mkbuf`, :func:`pvm_initsend`, :func:`pvm_recv`
