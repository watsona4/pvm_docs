:func:`pvm_lookup`
==================

.. function:: int pvm_lookup(char* name, int index, int* data)

   .. deprecated:: 3.4
      Relaced by :func:`pvm_getinfo`

   See :func:`pvm_insert` for a description of this database.

   :func:`pvm_lookup` retrieves data stored in the location given by
   <`name`, `index`>. If `index` is ``-1``, the data stored at the
   first existing index in the named class is returned.

   :param char* name: The class name, a null-terminated string.
   :param int index: The class index, ``>= 0`` or ``-1`` for first
      available.
   :param int* data: Returns the data stored in the <`name`, `index`>
      entry.
   :returns: The index at which the data was stored.
   :rtype: int

Fortran Call
------------

Not available

Errors
------

If successful, :func:`pvm_lookup` returns the index at which the data
was stored (``>= 0``), otherwise it returns a negative error code:

:macro:`PvmBadParam`
   giving an invalid argument value.

:macro:`PvmNoEntry`
   the requested <`name`, `index`> pair does not exist.

See Also
--------

:func:`pvm_delete`, :func:`pvm_insert`
