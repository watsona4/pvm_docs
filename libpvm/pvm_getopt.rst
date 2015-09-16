:func:`pvm_getopt`
==================

.. function:: int pvm_getopt(int what)

   The routine :func:`pvm_getopt` returns the value of the specified
   option in PVM. For a discussion of options and values, see
   :func:`pvm_setopt`. The valid options are:

   ============================= == =
   :macro:`PvmRoute`              1  Message routing policy
   :macro:`PvmDebugMask`          2  Libpvm debug mask
   :macro:`PvmAutoErr`            3  Auto error reporting
   :macro:`PvmOutputTid`          4  Stdout destination for children
   :macro:`PvmOutputCode`         5  Output message tag for children
   :macro:`PvmTraceTid`           6  Trace data destination for children
   :macro:`PvmTraceCode`          7  Trace message tag for children
   :macro:`PvmTraceBuffer`        8  Trace buffer size for children
   :macro:`PvmTraceOptions`       9  Trace collection options for children
   :macro:`PvmFragSize`          10  Message fragment size
   :macro:`PvmResvTids`          11  Allow messages to reserved tags and TIDs
   :macro:`PvmSelfOutputTid`     12  Stdout destination
   :macro:`PvmSelfOutputCode`    13  Output message tag
   :macro:`PvmSelfTraceTid`      14  Trace data destination
   :macro:`PvmSelfTraceCode`     15  Trace message tag
   :macro:`PvmSelfTraceBuffer`   16  Trace buffer size
   :macro:`PvmSelfTraceOptions`  17  Trace collection options
   :macro:`PvmShowTids`          18  :func:`pvm_catchout` prints task ids with output
   :macro:`PvmPollType`          19  Message wait policy (shared memory)
   :macro:`PvmPollTime`          20  Message spinwait duration
   :macro:`PvmOutputContext`     21  Output message context for children
   :macro:`PvmTraceContext`      22  Trace message context for children
   :macro:`PvmSelfOutputContext` 23  Output message context
   :macro:`PvmSelfTraceContext`  24  Trace message context
   :macro:`PvmNoReset`           25  Do not kill task on reset
   ============================= == =

   If an error occurs, the PVM error code is returned in place of the
   option value.

   :param int what: Integer defining option to get.
   :returns: Integer returning the value of the option.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfgetopt(what, val)

   :param integer what: Integer defining option to get.
   :param integer val: Integer returning the value of the option.

Examples
--------

C:

.. code-block:: c

   val = pvm_getopt( PvmFragSize );

Fortran:

.. code-block:: fortran

   CALL PVMFGETOPT( PVMAUTOERR, VAL )

Errors
------

This error condition can be returned:

:macro:`PvmBadParam`
   giving an invalid value.

See Also
--------

:func:`pvm_setopt`
