:func:`pvm_pack`
================

.. function:: int pvm_packf(const char* fmt, ...)

.. function:: int pvm_pkbyte(char* xp, int nitem, int stride)

.. function:: int pvm_pkcplx(float* cp, int nitem, int stride)

.. function:: int pvm_pkdcplx(double* zp, int nitem, int stride)

.. function:: int pvm_pkdouble(double* dp, int nitem, int stride)

.. function:: int pvm_pkfloat(float* fp, int nitem, int stride)

.. function:: int pvm_pkint(int* ip, int nitem, int stride)

.. function:: int pvm_pkuint(unsigned int* ip, int nitem, int stride)

.. function:: int pvm_pkushort(unsigned short* ip, int nitem, int stride)

.. function:: int pvm_pkulong(unsigned long* ip, int nitem, int stride)

.. function:: int pvm_pklong(long* ip, int nitem, int stride)

.. function:: int pvm_pkshort(short* jp, int nitem, int stride)

.. function:: int pvm_pkstr(char* sp)

   Each of the :func:`pvm_pk`\ * routines packs an array of the given
   data type into the active send buffer. The arguments for each of
   the routines are a pointer to the first item to be packed, `nitem`
   which is the total number of items to pack from this array, and
   `stride` which is the stride to use when packing.

   An exception is :func:`pvm_pkstr` which by definition packs a
   ``NULL`` terminated character string and thus does not need `nitem`
   or `stride` arguments. The Fortran routine
   :f:func:`pvmfpack(STRING, ...) <pvmfpack>` expects `nitem` to be
   the number of characters in the string and `stride` to be ``1``.

   A null string (``""``) can be packed; this is just a string with no
   characters before the terminating ``'\0'``. However, packing a null
   string pointer, ``(char*)0``, is not allowed.

   If the packing is successful, `info` will be ``0``. If some error
   occurs then `info` will be ``< 0``.

   A single variable (not an array) can be packed by setting ``nitem =
   1`` and ``stride = 1``.

   The routine :func:`pvm_packf` uses a :func:`printf`\ -like format
   expression to specify what and how to pack data into the send
   buffer. All variables are passed as addresses if `count` and
   `stride` are specified otherwise, variables are assumed to be
   values. A BNF-like description of the format syntax is::

     format : null | init | format fmt
     init : null | '%' '+'
     fmt : '%' count stride modifiers fchar
     fchar : 'c' | 'd' | 'f' | 'x' | 's'
     count : null | [0-9]+ | '*'
     stride : null | '.' ( [0-9]+ | '*' )
     modifiers : null | modifiers mchar
     mchar : 'h' | 'l' | 'u'

   Formats:
     * ``+``: means initsend - must match an int (how) in the param list.
     * ``c``: pack/unpack bytes
     * ``d``: integers
     * ``f``: float
     * ``x``: complex float
     * ``s``: string

   Modifiers:
     * ``h``: short (int)
     * ``l``: long (int, float, complex float)
     * ``u``: unsigned (int)

   Future extensions to the `what` argument in :f:func:`pvmfpack` will
   include 64 bit types when XDR encoding of these types is available.
   Meanwhile users should be aware that precision can be lost when
   passing data from a 64 bit machine like a Cray to a 32 bit machine
   like a SPARCstation. As a mnemonic the `what` argument name
   includes the number of bytes of precision to expect. By setting
   encoding to PVMRAW (see :f:func:`pvmfinitsend`) data can be
   transferred between two 64 bit machines with full precision even if
   the PVM configuration is heterogeneous.

   Messages should be unpacked exactly like they were packed to ensure
   data integrity. Packing integers and unpacking them as floats will
   often fail because a type encoding will have occurred transferring
   the data between heterogeneous hosts. Packing 10 integers and 100
   floats then trying to unpack only 3 integers and the 100 floats
   will also fail.

   :param char* fmt: Printf-like format expression specifying what to
      pack.
   :param int nitem: The total number of items to be packed (not the
      number of bytes).
   :param int stride: The stride to be used when packing the items.
      For example, if ``stride = 2`` in :func:`pvm_pkcplx`, then every
      other complex number will be packed.
   :param char* xp: Pointer to the beginning of a block of bytes. Can
      be any data type, but must match the corresponding unpack data
      type.
   :param float* cp: Complex array at least ``nitem*stride`` items
      long.
   :param double* zp: Double precision complex array at least
      ``nitem*stride`` items long.
   :param double* dp: Double precision real array at least
      ``nitem*stride`` items long.
   :param float* fp: Real array at least ``nitem*stride`` items long.
   :param int* ip: Integer array at least ``nitem*stride`` items long.
   :param short* jp: Integer``*2`` array at least ``nitem*stride``
      items long.
   :param char* sp: Pointer to a null terminated character string.
   :returns: Integer status code returned by the routine. Values less
      than zero indicate an error.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfpack(what, xp, nitem, stride, info)

   The type of data for `what` are:

   ================= = ================== =
   :macro:`STRING`   0 :macro:`REAL4`     4
   :macro:`BYTE1`    1 :macro:`COMPLEX8`  5
   :macro:`INTEGER2` 2 :macro:`REAL8`     6
   :macro:`INTEGER4` 3 :macro:`COMPLEX16` 7
   ================= = ================== =

   :param integer what: Integer specifying the type of data being
      packed.
   :param character xp: Pointer to the beginning of a block of
      bytes. Can be any data type, but must match the corresponding
      unpack data type.
   :param integer nitem: The total number of items to be packed (not
      the number of bytes).
   :param integer stride: The stride to be used when packing the
      items. For example, if ``stride = 2`` in :func:`pvm_pkcplx`,
      then every other complex number will be packed.
   :param integer info: Integer status code returned by the
      routine. Values less than zero indicate an error.

Examples
--------

C:

.. code-block:: c

   info = pvm_initsend(PvmDataDefault);
   info = pvm_pkstr("initial data");
   info = pvm_pkint(&size, 1, 1);
   info = pvm_pkint(array, size, 1);
   info = pvm_pkdouble(matrix, size*size, 1);
   msgtag = 3;
   info = pvm_send(tid, msgtag);

   int count, *iarry;
   double darry[4];
   pvm_packf("%+ %d %*d %4lf", PvmDataRaw, count, count, iarry, darry);

Fortran:

.. code-block:: fortran

   CALL PVMFINITSEND(PVMRAW, INFO)
   CALL PVMFPACK(INTEGER4, NSIZE, 1, 1, INFO)
   CALL PVMFPACK(STRING, 'row 5 of NXN matrix', 19, 1, INFO)
   CALL PVMFPACK(REAL8, A(5,1), NSIZE, NSIZE , INFO)
   CALL PVMFSEND(TID, MSGTAG, INFO)

.. warning::

   Strings cannot be packed when using the :macro:`PvmDataInPlace`
   encoding, due to limitations in the implementation. Attempting to
   pack a string using :func:`pvm_pkstr` or :func:`pvm_packf` will
   cause error code :macro:`PvmNotImpl` to be returned.

Errors
------

:macro:`PvmNoMem`
   Malloc has failed. Message buffer size has exceeded the available
   memory on this host.

:macro:`PvmNoBuf`
   There is no active send buffer to pack into. Try calling
   :func:`pvm_initsend` before packing message.

:macro:`PvmOverflow`
   Attempt to pack a value too large, e.g., packing an 8-byte long
   with XDR encoding if the value won't fit into 4 bytes.

See Also
--------

:func:`pvm_initsend`, :func:`pvm_unpack`, :func:`pvm_send`,
:func:`pvm_recv`, :func:`pvm_pkmesg`
