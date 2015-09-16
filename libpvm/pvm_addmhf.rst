:func:`pvm_addmhf`, :func:`pvm_delmhf`
======================================

.. function:: int pvm_addmhf(int src, int tag, int ctx, int (*func)(int mid))

   :func:`pvm_addmhf` specifies a function that will be called
   whenever :ref:`libpvm <libpvm>` copies in a message whose header
   fields of `src`, `tag`, and `ctx` match those provided to
   :func:`pvm_addmhf`.

   The `src` and `tag` fields may be left unspecified (wildcard) by
   setting to ``-1``.

   The calling sequence of the message handler function is:

   .. code-block:: c

      int handler(int mid)

   Where `mid` is the `bufid` of the received message. The handler
   function can be used to unpack and process the received message
   buffer. PVM automatically saves the current send and receive
   buffers, so the handler need not worry about interfering with
   message buffers in the regular program flow.  PVM also sets the
   current receive buffer to the received message (using
   :func:`pvm_setrbuf`) before invoking the message handler, so the
   message can be unpacked directly. PVM will free this message buffer
   when the message handler returns, if the handler has not already
   done so. But, any other message buffers created by the handler
   routine should be freed using :func:`pvm_freebuf` before returning.

   .. note:: 

      Operation in the message handler context is somewhat restricted.
      The function may call some PVM functions, but not others. For
      example, it may compose and send a reply message as shown:

      .. code-block:: c

	 pvm_packf("%+ %s", PvmDataDefault, "got your message");
         pvm_send(tid, tag);
         pvm_freebuf(pvm_setsbuf(0));

      or equivalently:

      .. code-block:: c

	 pvm_setsbuf(pvm_mkbuf(PvmDataDefault));
         pvm_pkstr("got your message");
         pvm_send(tid, tag);
         pvm_freebuf(pvm_setsbuf(0));

      but is not allowed to call certain other PVM communication
      functions, such as multicast or receive.

   :func:`pvm_addmhf` returns the ID number of the newly created
   message handler if successful; this number may be passed to
   :func:`pvm_delmhf` to remove the entry. There is no guarantee to
   the ordering of ID values returned by :func:`pvm_addmhf`, or to the
   order in which message handlers will be invoked. :macro:`PvmExists`
   is returned if the handler already exists.

   :param int src: The tid of the sender.
   :param int tag: The tag sent with the message.
   :param int ctx: The context sent with the message.
   :param int* func: Function to call when message received.
   :param int mid: Message buffer identifier for new active receive
      buffer.
   :returns: Result code.
   :rtype: int

.. function:: int pvm_delmhf(int mhid)

   :func:`pvm_delmhf` removes a message handler function that was
   added by :func:`pvm_addmhf`. :func:`pvm_delmhf` returns
   :macro:`PvmOk` if successful, :macro:`PvmBadParam` if
   :func:`pvm_delmhf` is passed a negative ID
   value, or :macro:`PvmNotFound` if the ID value is not found.

   :param int mhid: Message handler ID.
   :returns: Result code.
   :rtype: int

Fortran Call
------------

Not available

Examples
--------

.. code-block:: c

   /* Print a message when hosts are added to virtual machine */

   int
   hostAdded(int mid)
     {
       int n;
       pvm_unpackf("%d", &n);
       printf("*** %d new hosts just added ***\n", n);
     }

   void
   main()
     {
       int src, tag, ctx;

       . . .

       src = -1;
       tag = 99;
       ctx = -1;

       pvm_addmhf(src, tag, ctx, hostAdded);
       pvm_notify(PvmHostAdd, 99, -1, (int *) NULL);

       . . .
     }

Errors
------

The following error conditions can be returned by :func:`pvm_addmhf`:

:macro:`PvmExists`
   Can't insert as handler already exists with same (`tag`, `ctx`,
   `src`) including "wild-cards" (those set to ``-1``)

The following error conditions can be returned by :func:`pvm_delmhf`:

:macro:`PvmBadParam`
   Invalid (negative) mhid passed in.

:macro:`PvmNotFound`
   Message handler mhid does not exist.

See Also
--------

:func:`pvm_setrbuf`, :func:`pvm_setsbuf`, :func:`pvm_freebuf`
