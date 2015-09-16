:func:`pvm_delhosts`
====================

.. function:: int pvm_delhosts(char** hosts, int nhost, int* infos)

   The routine :func:`pvm_delhosts` deletes the computers pointed to
   in `hosts` from the existing configuration of computers making up
   the virtual machine. All PVM processes and the :ref:`pvmd <pvmd3>`
   running on these computers are killed as the computer is deleted.
   If :func:`pvm_delhosts` is successful, `info` will be
   `nhost`. Partial success is indicated by ``1 <= info < nhost``, and
   total failure by ``info < 1``. The array `infos` can be checked to
   determine which host caused the error.

   If a host fails, the PVM system will continue to function and will
   automatically delete this host from the virtual machine.  An
   application can be notified of a host failure by calling
   :func:`pvm_notify`.  It is still the responsibility of the
   application developer to make his application tolerant of host
   failure.

   :param char** hosts: An array of pointers to character strings
      containing the names of the machines to be deleted.
   :param int nhost: Integer specifying the number of hosts to be
      deleted.
   :param int* infos: Integer array of length `nhost` which contains
      the status code returned by the routine for the individual
      hosts. Values less than zero indicate an error.
   :returns: Integer status code returned by the routine. Values less
      than `nhost` indicate partial failure, values less than ``1``
      indicate total failure.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfdelhost(host, info)

   The Fortran routine :f:func:`pvmfdelhost` deletes a single host
   from the configuration with each call.

   :param character host: Character string containing the name of the
      machine to be deleted.
   :param integer info: Integer status code returned by the routine.
      Values less than `nhost` indicate partial failure, values less
      than ``1`` indicate total failure.

Examples
--------

C:

.. code-block:: c

   static char *hosts[] = {
       "sparky",
       "thud.cs.utk.edu",
   };
   int status[2];
   info = pvm_delhosts(hosts, 2, status);

Fortran:

.. code-block:: fortran

   CALL PVMFDELHOST('azure', INFO)

Errors
------

These error conditions can be returned by :func:`pvm_delhosts`:

:macro:`PvmBadParam`
   giving an invalid argument value.

:macro:`PvmSysErr`
   local :ref:`pvmd <pvmd3>` not responding.

See Also
--------

:func:`pvm_addhosts`, :func:`pvm_notify`
