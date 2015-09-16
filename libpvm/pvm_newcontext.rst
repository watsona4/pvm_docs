:func:`pvm_newcontext`, :func:`pvm_setcontext`, :func:`pvm_freecontext`, :func:`pvm_getcontext`
===============================================================================================

The context functions provide a system-wide unique context and the
means to manipulate this context.

Contexts provide the ability for communicating tasks to automatically
differentiate messages by the context in which they were sent. Thus a
message sent in context **A** by the sender must be received in
context **A** by the recipient. A sender may send in any
context. However, a recipient will not accept a message sent in a
context that differs from its own.

One such use of contexts is with library routines. Using contexts,
library routine inter-communication may be logically seperated from
the user's application inter-communication. This will prevent the
inadvertent receipt of one another's messages.

Spawned tasks inherit the spawn-time context of their parent.
Existing PVM applications work unchanged using the default context.

.. function:: int pvm_newcontext(void)

   :func:`pvm_newcontext` returns a newly allocated context. However,
   this new context is not yet active.

   :returns: New context value.
   :rtype: int

.. function:: int pvm_setcontext(int new_ctx)

   :func:`pvm_setcontext` changes the current context from `old_ctx`
   to `new_ctx`.

   :param int new_ctx: New context value.
   :returns: Prior context value.
   :rtype: int

.. function:: int pvm_freecontext(int ctx)

   :func:`pvm_freecontext` frees `ctx` so that it may be reused.
   Contexts are a system resource that will be exhausted if not
   recycled.

   :param int ctx: Context value.
   :returns: Result code.
   :rtype: int

.. function:: int pvm_getcontext(void)

   :func:`pvm_getcontext` returns the current context of the
   requesting task.

   :returns: Current context value.
   :rtype: int

Fortran Call
------------

.. f:subroutine:: pvmfnewcontext(ctx)

   :param integer ctx: Context value.

.. f:subroutine:: pvmfsetcontext(new_ctx, old_ctx)

   :param integer new_ctx: New context value.
   :param integer old_ctx: Prior context value.

.. f:subroutine:: pvmffreecontext(ctx, info)

   :param integer ctx: Context value.
   :param integer info: Result code.

.. f:subroutine:: pvmfgetcontext(ctx)

   :param integer ctx: Context value.

Examples
--------

.. code-block:: c

   /* parent task with context */
     int cc, context0, context1;
     char buf[25];

     context0 = pvm_getcontext();       /*  get my current context */
     context1 = pvm_newcontext();       /*  get a new context */
     pvm_setcontext(context1);          /*  set my context to new context */
     printf("My context is: %d", context1);
     pvm_spawn("child", (char**)0, PvmTaskDefault, "", 1, &tid);
     cc = pvm_recv(-1, -1);             /*  receive message from child - in context1 */
     pvm_upkstr(buf);
     printf("%s", buf);
     pvm_setcontext(context0);          /*  reset my context to my original context0 */

   /* child task with context - child inherits parent's context as default */
     int context;
     int ptid;
     char buf[25];

     ptid = pvm_parent();
     context = pvm_getcontext();        /*  get my current context */
     sprintf(buf, "Greetings from child who's context is: %d.", context);
     pvm_initsend(PvmDataDefault);
     pvm_pkstr(buf);
     pvm_send(ptid, 1);

Errors
------

Only system resource errors will be returned as the context programs
themselves do not generate errors.
