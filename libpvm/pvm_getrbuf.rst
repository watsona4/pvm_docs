:func:`pvm_getrbuf`
===================

.. function:: int pvm_getrbuf(void)

   The routine :func:`pvm_getrbuf` returns the message buffer
   identifier `bufid` for the active receive buffer or ``0`` if there
   is no current buffer.

   :returns: Integer returning message buffer identifier for the
      active receive buffer.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfgetrbuf(bufid)

   :param integer bufid: Integer returning message buffer identifier
      for the active receive buffer.

Examples
--------

C:

.. code-block:: c

   bufid = pvm_getrbuf();

Fortran:

.. code-block:: fortran

   CALL PVMFGETRBUF(BUFID)

Errors
------

No error conditions are returned by :func:`pvm_getrbuf`.

See Also
--------

:func:`pvm_getsbuf`
