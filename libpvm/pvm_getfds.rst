:func:`pvm_getfds`
==================

.. function:: int pvm_getfds(int** fds )

   A PVM task uses sockets to communicate between :ref:`libpvm
   <libpvm>` and other tasks or the :ref:`pvmd <pvmd3>`. It is
   sometimes useful to know the file descriptor numbers of the sockets
   in order to wait from input from either PVM messages or an external
   source. For example, the PVM console waits on both keyboard input
   and notify messages. Input can be multiplexed by polling all
   sources, but this wastes CPU cycles. Instead, the :func:`select`
   system call can be used to wait until one or more sources of input
   are ready.

   If it completes successfully, :func:`pvm_getfds` returns the number
   of sockets in use, and the file descriptor numbers in an array
   (allocated and freed by :ref:`libpvm <libpvm>`). At least one
   socket always exists (from task to :ref:`pvmd <pvmd3>`), and its
   descriptor is always ``fds[0]``. The number of sockets varies as
   direct routes are established to other tasks.

   It can be difficult to track the set of file descriptors if direct
   routing is enabled, because routes are created as messages are
   either sent or received. The simplest approach is to disable direct
   routing.

   When select returns with a PVM file descriptor ready, a complete
   message may be ready to be received, or instead only a fragment may
   be waiting. :func:`pvm_nrecv` or :func:`pvm_probe` should be used
   test without blocking.

   :param int** fds: Returns integer array of file descriptors.

Restrictions
------------

:func:`pvm_getfds` is only available when running PVM on a Unix or
similar system.

Examples
--------

The following program fragment waits until either keyboard input is
available, or a PVM message has arrived.

.. code-block:: c

   int *d;
   fd_set r;

   pvm_setopt(PvmRoute, PvmDontRoute);
   pvm_getfds(&d);

   FD_ZERO(&r);
   FD_SET(0, &r);
   FD_SET(d[0], &r);
   while (1) {
       if (select(d[0] + 1, &r, (fd_set*)0, (fd_set*)0,
                  (struct timeval*)0) > 0) {
           if (FD_ISSET(0, &r))
             ...    /* read keyboard input */
           if (FD_ISSET(d[0], &r) && pvm_nrecv(-1, -1) > 0)
             ...    /* got a PVM message */
         }
     }

Errors
------

The following error condition can be returned by :func:`pvm_getfds`:

:macro:`PvmSysErr`
   :ref:`pvmd <pvmd3>` not responding.

See Also
--------

:func:`pvm_notify`, :func:`pvm_trecv`
