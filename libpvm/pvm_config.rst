:func:`pvm_config`
==================

.. function:: int pvm_config(int* nhost, int* narch, struct pvmhostinfo **hostp)

   The routine :func:`pvm_config` returns information about the
   present virtual machine. The information returned is similar to
   that available from the console command :command:`conf`.

   The C function returns information about the entire virtual machine
   in one call. Note that in C the `hostp` array is allocated and owned by
   :ref:`libpvm <libpvm>`. It is automatically freed or reused on the
   next call to :func:`pvm_config`.

   If :func:`pvm_config` is successful, `info` will be ``0``.  If some
   error occurs then `info` will be ``< 0``.

   :param int* nhost: Integer returning the number of hosts
      (:ref:`pvmd <pvmd3>`\ s) in the virtual machine.
   :param int* narch: Integer returning the number of different data
      formats being used.
   :param pvmhostinfo** hostp: Returns pointer to an array of
      structures which contain information about each host including
      its pvmd task ID, name, architecture, and relative speed.
   :returns:  Integer status code returned by the routine. Values less than
      zero indicate an error.
   :rtype: int

The PVM host configration information is stored in the
:type:`pvmhostinfo` structure:

.. type:: struct pvmhostinfo

   .. member:: int pvmhostinfo.hi_tid

      Integer :ref:`pvmd <pvmd3>` task ID for host

   .. member:: char* pvmhostinfo.hi_name

      Character string name of host

   .. member:: char* pvmhostinfo.hi_arch

      Character string architecture name of host

   .. member:: int pvmhostinfo.hi_speed

      Integer relative speed of host. Default value is 1000.

Fortran Call
------------

.. f:subroutine:: pvmfconfig(nhost, narch, dtid, name, arch, speed, info)

   The Fortran function returns information about one host per call
   and cycles through all the hosts. Thus, if :f:func:`pvmfconfig` is
   called `nhost` times, the entire virtual machine will be
   represented.

   Note that in Fortran the reported value of `nhost` and the host
   configuration do not change until the function resets at the end of
   a complete cycle. The user can reset :f:func:`pvmfconfig` at any
   time by calling it with ``nhost = -1``.

   :param integer nhost: Integer returning the number of hosts
      (:ref:`pvmd <pvmd3>`\ s) in the virtual machine.
   :param integer narch: Integer returning the number of different
      data formats being used.
   :param integer dtid: Integer returning pvmd task ID for host
   :param character name: Character string returning name of host
   :param character arch: Character string returning architecture name
      of host
   :param integer speed: Integer returning relative speed of host.
      Default value is 1000.
   :param integer info: Integer status code returned by the routine.
      Values less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   struct pvmhostinfo *hostp;
   int i, nhost, narch;

   info = pvm_config(&nhost, &narch, &hostp);
   for (i = 0; i < nhost; i++)
     printf("%s\n", hostp[i].hi_name);

Fortran:

.. code-block:: fortran

   Do i=1, NHOST
     CALL PVMFCONFIG(NHOST, NARCH, DTID(i), HOST(i), ARCH(i), SPEED(i), INFO)
   Enddo

Errors
------

The following error condition can be returned by :func:`pvm_config`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding.

See Also
--------

:func:`pvm_tasks`
