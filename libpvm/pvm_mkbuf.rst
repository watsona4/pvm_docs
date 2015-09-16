:func:`pvm_mkbuf`
=================

.. function:: int pvm_mkbuf(int encoding)

   The routine :func:`pvm_mkbuf` creates a new message buffer and sets
   its encoding status to `encoding`. If :func:`pvm_mkbuf` is
   successful, `bufid` will be the identifier for the new buffer,
   which can be used as a send buffer. If some error occurs then
   `bufid` will be ``< 0``.

   With the default setting XDR encoding is used when packing the
   message because PVM cannot know if the user is going to add a
   heterogeneous machine before this message is sent. The other
   options to encoding allow the user to take advantage of knowledge
   about his virtual machine even when it is heterogeneous. For
   example, if the user knows that the next message will only be sent
   to a machine that understands the native format, then he can use
   :macro:`PvmDataRaw` encoding and save on encoding costs.

   :macro:`PvmDataInPlace` encoding specifies that data be left in
   place during packing. The message buffer only contains the sizes
   and pointers to the items to be sent. When :func:`pvm_send` is
   called the items are copied directly out of the user's memory. This
   option decreases the number of times a message is copied at the
   expense of requiring the user to not modify the items between the
   time they are packed and the time they are sent.

   :func:`pvm_mkbuf` is required if the user wishes to manage multiple
   message buffers and should be used in conjunction with
   :func:`pvm_freebuf`. :func:`pvm_freebuf` should be called for a
   send buffer after a message has been sent and is no longer needed.

   Receive buffers are created automatically by the :func:`pvm_recv`
   and :func:`pvm_nrecv` routines and do not have to be freed unless
   they have been explicitly saved with :func:`pvm_setrbuf`.

   Typically multiple send and receive buffers are not needed and the
   user can simply use the :func:`pvm_initsend` routine to reset the
   default send buffer.

   There are several cases where multiple buffers are useful. One
   example where multiple message buffers are needed involves
   libraries or graphical interfaces that use PVM and interact with a
   running PVM application but do not want to interfere with the
   application's own communication.

   When multiple buffers are used they generally are made and freed
   for each message that is packed.

   Encoding options in C are:

   ======================= ===== ==================
   Encoding                Value Meaning
   ======================= ===== ==================
   :macro:`PvmDataDefault` 0     XDR
   :macro:`PvmDataRaw`     1     no encoding
   :macro:`PvmDataInPlace` 2     data left in place
   ======================= ===== ==================

   :param int encoding: Integer specifying the buffer's encoding
      scheme.
   :returns: Integer message buffer identifier returned. Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfmkbuf(encoding, bufid)

   =================== ===== ==================
   Encoding            Value Meaning
   =================== ===== ==================
   :macro:`PVMDEFAULT` 0     XDR
   :macro:`PVMRAW`     1     no encoding
   :macro:`PVMINPLACE` 2     data left in place
   =================== ===== ==================

   :param integer encoding: Integer specifying the next message's
      encoding scheme.
   :param integer bufid: Integer returned containing the message
      buffer identifier. Values less than zero indicate an error.

.. warning::

   :macro:`PvmDataInPlace` allows only dense (stride = 1) data in
   version 3.3. It cannot be used on shared memory (\*MP)
   architectures; a :macro:`PvmNotImpl` error will occur at send time.

Examples
--------

C:

.. code-block:: c

   bufid = pvm_mkbuf(PvmDataRaw);
   /* send message */
   info = pvm_freebuf(bufid);

Fortran:

.. code-block:: fortran

   CALL PVMFMKBUF(PVMDEFAULT, MBUF)
   ! SEND MESSAGE HERE
   CALL PVMFFREEBUF(MBUF, INFO)

Errors
------

These error conditions can be returned by :func:`pvm_mkbuf`:

:macro:`PvmBadParam`
   giving an invalid encoding value

:macro:`PvmNoMem`
   Malloc has failed. There is not enough memory to create the buffer

See Also
--------

:func:`pvm_initsend`, :func:`pvm_freebuf`
