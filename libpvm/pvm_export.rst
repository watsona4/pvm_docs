:func:`pvm_export`, :func:`pvm_unexport`
========================================

.. function:: int pvm_export(char* name)

.. function:: int pvm_unexport(char* name)

   The routines :func:`pvm_export` and :func:`pvm_unexport` are
   provided for convenience in editing environment variable
   :envvar:`PVM_EXPORT`, while maintaining the colon-separated list
   syntax it requires. The variable can be modified by other means,
   and at the same time by the :func:`pvm_export` functions.

   :func:`pvm_export` checks to see if a name is already in
   :envvar:`PVM_EXPORT` before including it, and exporting a name more
   than once is not considered an error. Likewise,
   :func:`pvm_unexport` will not complain if you specify a name not in
   :envvar:`PVM_EXPORT`.

   :param char* name: Name of an environment variable to add to or
      delete from export list.

Examples
--------

.. code-block:: c

   /* PVM_EXPORT=SHELL:HOME */
   pvm_export("DISPLAY");
   pvm_export("TERM");
   pvm_unexport("HOME");
   /* PVM_EXPORT=SHELL:DISPLAY:TERM */

Errors
------

No error conditions are currently returned by :func:`pvm_export` and
:func:`pvm_unexport`.

See Also
--------

:ref:`pvm <pvm>`, :func:`pvm_spawn`
