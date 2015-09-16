:func:`pvm_initsend`
====================

.. function:: int pvm_initsend(int encoding)

   The routine :func:`pvm_initsend` clears the send buffer and
   prepares it for packing a new message. The encoding scheme used for
   the packing is set by `encoding`. XDR encoding is used by default
   because PVM can not know if the user is going to add a
   heterogeneous machine before this message is sent. If the user
   knows that the next message will only be sent to a machine that
   understands the native format, then he can use :macro:`PvmDataRaw`
   encoding and save on encoding costs.

   :macro:`PvmDataInPlace` encoding specifies that data be left in
   place during packing. The message buffer only contains the sizes
   and pointers to the items to be sent. When :func:`pvm_send` is
   called the items are copied directly out of the user's memory. This
   option decreases the number of times a message is copied at the
   expense of requiring the user to not modify the items between the
   time they are packed and the time they are sent.

   If :func:`pvm_initsend` is successful, then `bufid` will contain
   the message buffer identifier. If some error occurs then `bufid`
   will be ``< 0``.

   Encoding options in C are:

   ======================= ===== ==================
   Encoding                Value Meaning
   ======================= ===== ==================
   :macro:`PvmDataDefault` 0     XDR
   :macro:`PvmDataRaw`     1     no encoding
   :macro:`PvmDataInPlace` 2     data left in place
   ======================= ===== ==================

   :param int encoding: Integer specifying the next message's encoding
      scheme.
   :returns: Integer returned containing the message buffer
      identifier. Values less than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfinitsend(encoding, bufid)

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

   bufid = pvm_initsend(PvmDataDefault);
   info = pvm_pkint(array, 10, 1);
   msgtag = 3;
   info = pvm_send(tid, msgtag);

Fortran:

.. code-block:: fortran

   CALL PVMFINITSEND(PVMRAW, BUFID)
   CALL PVMFPACK(REAL4, DATA, 100, 1, INFO)
   CALL PVMFSEND(TID, 3, INFO)

Errors
------

These error conditions can be returned by :func:`pvm_initsend`:

:macro:`PvmBadParam`
   giving an invalid encoding value

:macro:`PvmNoMem`
   Malloc has failed. There is not enough memory to create the buffer

See Also
--------

:func:`pvm_mkbuf`
