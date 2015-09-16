:func:`pvm_addhosts`
--------------------

.. function:: int pvm_addhosts(char** hosts, int nhost, int* infos)

   The routine :func:`pvm_addhosts` adds the computers named in
   `hosts` to the configuration of computers making up the virtual
   machine. The names should have the same syntax as lines of a
   :ref:`pvmd <pvmd3>` hostfile (see page for :ref:`pvmd3
   <pvmd3>`): A hostname followed by options of the form ``xx=y``.

   If :func:`pvm_addhosts` is successful, `info` will be equal to
   `nhost`. Partial success is indicated by ``0 < info < nhost``, and
   total failure by info ``< 1``. The array `infos` can be checked to
   determine which host caused the error.

   The Fortran routine pvmfaddhost adds a single host to the
   configuration with each call. `Info` will be ``1`` if successful or
   ``< 0`` if error.

   The status of hosts can be requested by the application using
   :func:`pvm_mstat` and :func:`pvm_config`. If a host fails it will
   be automatically deleted from the configuration. Using
   :func:`pvm_addhosts` a replacement host can be added by the
   application, however it is the responsibility of the application
   developer to make his application tolerant of host failure.
   Another use of this feature would be to add more hosts as they
   become available, for example on a weekend, or if the application
   dynamically determines it could use more computational power.

   :param char** hosts: An array of strings naming the hosts to be
      added. :ref:`Pvmd <pvmd3>` must already be installed and the
      user must have an account on the specified hosts.
   :param int nhost: Integer specifying the length of array hosts.
   :param int* infos: Integer array of length `nhost` which returns
      the status for each host. Values less than zero indicate an
      error, while positive values are TIDs of the new hosts.
   :param char* host: Character string naming the host to be added.
   :returns: Integer status code returned by the routine. Values less
      than `nhost` indicate partial failure, and values less than
      ``1`` indicate total failure.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfaddhost(host, info)

Examples
--------

C:

.. code-block:: c

   static char *hosts[] = {
      "sparky",
      "thud.cs.utk.edu",
   };
   info = pvm_addhosts(hosts, 2, infos);

Fortran:

.. code-block:: fortran

   CALL PVMFADDHOST('azure', INFO)

Errors
------

The following error conditions can be returned by
:func:`pvm_addhosts`:

:macro:`PvmBadParam`
   giving an invalid argument value.

:macro:`PvmAlready`
   another task is currently adding hosts.

:macro:`PvmSysErr`
   local :ref:`pvmd <pvmd3>` is not responding.

and in the `infos` vector:

:macro:`PvmBadParam`
   bad hostname syntax.

:macro:`PvmNoHost`
   no such host.

:macro:`PvmCantStart`
   failed to start :ref:`pvmd <pvmd3>` on host.

:macro:`PvmDupHost`
   host already configured.

:macro:`PvmBadVersion`
   :ref:`pvmd <pvmd3>` protocol versions don't match.

:macro:`PvmOutOfRes`
   PVM has run out of system resources.

See Also
--------

:ref:`pvmd3 <pvmd3>`, :func:`pvm_delhosts`, :func:`pvm_start_pvmd`,
:func:`pvm_config`, :func:`pvm_mstat`
