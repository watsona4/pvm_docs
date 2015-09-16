:func:`pvm_advise`
==================

.. function:: int pvm_advise(int route)

   .. deprecated:: 3.2
      Replaced by :func:`pvm_setopt`

   The routine :func:`pvm_advise` advises PVM on whether or not to set
   up direct task-to-task links (using TCP) for all subsequent
   communication. Once a link is established it remains until the
   application finishes. If a direct link can not be established
   because one of the two tasks has requested :macro:`PvmDontRoute` or
   because no resources are available, then the default route through
   the PVM daemons is used. :func:`pvm_advise` can be called multiple
   times to selectively establish direct links, but is typically set
   only once near the beginning of each task. :macro:`PvmAllowDirect`
   is the default advise setting. This setting on task A allows other
   tasks to set up direct links to A. Once a direct link is
   established between tasks both tasks will use it for sending
   messages. :func:`pvm_advise` returns the error status in
   `info`. The performance of direct task-to-task links can be up to a
   factor of two better than the default route. The draw back is a
   lack of scalability of the direct links. Some versions of UNIX
   limit the number of links to no more than 30.

   :param int route: Integer advising PVM to set up direct
      task-to-task links. :macro:`PvmDontRoute` (1): Don't allow
      direct links to this task; :macro:`PvmAllowDirect` (2): Allow
      but don't request direct links; :macro:`PvmRouteDirect` (3):
      Request direct links.
   :returns: Integer returning error status.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfadvise(route, info)

Examples
--------

C:

.. code-block:: c

   info = pvm_advise(PvmRouteDirect);

Fortran:

.. code-block:: fortran

   CALL PVMFADVISE(PVMROUTEDIRECT, INFO)

Errors
------

This error condition can be returned by :func:`pvm_advise`.

:macro:`PvmBadParam`
   giving an invalid route value.

See Also
--------

:func:`pvm_setopt`
