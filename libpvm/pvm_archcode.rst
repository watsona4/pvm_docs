:func:`pvm_archcode`
====================

.. function:: int pvm_archcode(char *arch)

   The routine :func:`pvm_archcode` returns an integer given an
   architecture name. The code returned identifies machines with
   compatible binary data formats. For example, SUN4 and RS6K have the
   same code, while ALPHA has a different one (because a few datatypes
   have different sizes). This lets you know when you can get away
   with using :macro:`PvmDataRaw` instead of :macro:`PvmDataDefault`
   encoding to pass messages between tasks on two machines.

   Naturally, you shouldn't assume the values returned by
   :func:`pvm_archcode` are etched in stone; the numbers have no
   intrinsic meaning except that if two different arch names map to
   the same value then they're compatible.

   This routine is actually obsolete in the sense that the
   architecture codes returned are already available in the
   :member:`hi_dsig` field of the :type:`pvmhostinfo` structure
   returned by :func:`pvm_config`, as shown in the below example.
   The routine is maintained for backwards compatibility only.

   :param char* arch: Character string containing the architecture
      name.
   :returns: Integer returning architecture code.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfarchcode(arch, cod)

Examples
--------

C:

.. code-block:: c

   struct pvmhostinfo *hip;
   int i;

   pvm_config((int *)0, (int *)0, &hip);
   i = pvm_archcode(hip[0].hi_arch);

   /* or you could just do:  i = hip[0].hi_dsig;  */

Fortran:

.. code-block:: fortran

   CALL PVMFARCHCODE('RS6K', k)

Errors
------

On success, :func:`pvm_archcode` returns a positive integer data
signature.

The following error conditions can be returned as well:

:macro:`PvmBadParam`
   giving an invalid architecture name.

:macro:`PvmNotFound`
   there is no host with the given architecture name in the current
   virtual machine configuration.

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding.

See Also
--------

:func:`pvm_config`, :func:`pvm_initsend`, :func:`pvm_notify`,
:func:`pvm_tasks`, :func:`pvm_tidtohost`
