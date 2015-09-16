:func:`pvm_delete`
==================

.. function:: int pvm_delete(char* name, int index)

   .. deprecated:: 3.4
      Relaced by :func:`pvm_delinfo`

   :func:`pvm_delete` deletes entry <`name`, `index`> from the
   database. See :func:`pvm_insert` for a description of this database.

   :param char* name: The class name, a null-terminated string.
   :param int index: The class index, ``>= 0``.
   :returns: If successful, :func:`pvm_delete` returns :macro:`PvmOk`,
      otherwise it returns a negative error code.
   :rtype: int

Fortran Call
------------

Not available

Errors
------

If successful, :func:`pvm_delete` returns :macro:`PvmOk`, otherwise it
returns a negative error code:

:macro:`PvmBadParam`
   giving an invalid argument value.

:macro:`PvmNoEntry`
   the requested <`name`, `index`> pair does not exist.

See Also
--------

:func:`pvm_insert`, :func:`pvm_lookup`
