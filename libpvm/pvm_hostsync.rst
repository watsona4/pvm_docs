:func:`pvm_hostsync`
====================

.. function:: int pvm_hostsync(int host, struct timeval* clk, struct timeval* delta)

   :func:`pvm_hostsync` samples the time-of day clock of a host in the
   virtual machine and returns both the clock value and the difference
   between local and remote clocks.

   To reduce the delta error due to message transit time, local clock
   samples are taken before and after reading the remote clock. Delta
   is the difference between the mean local clocks and remote clock.

   Note that the delta time can be negative. The microseconds field is
   always normalized to ``0..999999``, while the sign of the seconds
   field gives the sign of the delta. For example::

     3.25 Sec =  3,250000
     0        =  0,0
     -1 uSec  = -1,999999
     -1 Sec   = -1,000000
     -1.1 Sec = -2,999000

   In C, if `clk` or `delta` is a null pointer, that parameter is not
   returned.

   :param int host: TID of host.
   :param timeval* clk: Returns time-of-day clock sample from host.
   :param timeval* delta: Returns difference between local clock and
      remote host clock.

Fortran Call
------------

.. f:subroutine:: pvmfhostsync(host, clksec, clkusec, deltasec, deltausec, info)

   :param integer host: TID of host.
   :param integer clksec: Returns time-of-day seconds clock sample
      from host.
   :param integer clkusec: Returns time-of-day microseconds clock
      sample from host.
   :param integer deltasec: Returns difference between local clock and
      remote host clock seconds.
   :param integer deltausec: Returns difference between local clock
      and remote host clock microseconds.

Errors
------

If :func:`pvm_hostsync` is successful, it returns
:macro:`PvmOk`. These error conditions can be returned by
:func:`pvm_hostsync`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding.

:macro:`PvmNoHost`
   specified host not in virtual machine.

:macro:`PvmHostFail`
   host is unreachable (and thus possibly failed)

See Also
--------

:func:`pvm_config`
