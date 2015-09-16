:func:`pvm_getsbuf`
===================

.. function:: int pvm_getsbuf(void)

   The routine :func:`pvm_getsbuf` returns the message buffer
   identifier `bufid` for the active send buffer or ``0`` if there is
   no current buffer.

   :returns: Integer returning message buffer identifier for the
      active send buffer.
   :rtype: int

Fortran Call
------------

.. f:function:: pvmfgetsbuf(bufid)

   :param integer bufid: Integer returning message buffer identifier
      for the active send buffer.

Examples
--------

C:

.. code-block:: c

   bufid = pvm_getsbuf();

Fortran:

.. code-block:: fortran

   CALL PVMFGETSBUF(BUFID)

Errors
------

No error conditions are returned by :func:`pvm_getsbuf`.

See Also
--------

:func:`pvm_getrbuf`
