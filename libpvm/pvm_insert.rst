:func:`pvm_insert`
==================

.. function:: int pvm_insert(char* name, int index, int data)

   .. deprecated:: 3.4
      Relaced by :func:`pvm_putinfo`

   The master :ref:`pvmd <pvmd3>` maintains a simple database, which
   can be used to store values such as tids and make them accessible
   anywhere within a virtual machine. This is useful when building an
   application such as the group server, which must advertise its task
   id so clients can register send messages to register.

   The database stores integer data, indexed by <`name`, `index`>
   pairs. The `name` may be any null-terminated string and the index
   any non-negative integer. Database entries are grouped by name into
   classes; `index` may be specified as ``-1`` to store or retrieve
   the first available instance in a class.

   These functions are not part of the group library, but are the
   underlying mechanism used to implement it.

   :func:`pvm_insert` stores data at the given `index`. If `index` is
   ``-1``, the data is stored at the first available index in the
   named class, starting at ``0``.

   :param char* name: The class name, a null-terminated string.
   :param int index: The class index, ``>= 0`` or ``-1`` for first
      available.
   :param int data: Data to store in the <`name`, `index`> entry.
   :returns: The index at which the data was stored.
   :rtype: int

Fortran Call
------------

Not available

Errors
------

If successful, :func:`pvm_insert` returns the index at which the data
was stored, otherwise it returns a negative result. The following
error conditions can be returned:

:macro:`PvmBadParam`
   giving an invalid argument value.

:macro:`PvmDupEntry`
   the requested <`name`, `index`> pair is already in use.

See Also
--------

:func:`pvm_delete`, :func:`pvm_lookup`
