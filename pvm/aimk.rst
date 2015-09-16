.. _aimk:

Portable Make Wrapper Script
============================

.. program:: aimk

:ref:`aimk <aimk>` is a wrapper program for :command:`make`, used to
portably select options to build PVM and PVM applications on various
machines. Each port of PVM is assigned an architecture name. The name
is used both during compilation (to conditionally switch in code) and
at runtime (to select an executable or host).

:ref:`aimk <aimk>` uses the value of environment variable
:envvar:`PVM_ARCH` if it is set, otherwise it calls
:file:`$PVM_ROOT/pvmgetarch` to determine the architecture name.
:program:`pvmgetarch` is a script that sniffs at various parts of the
system to determine the correct architecture name. It is updated as
new PVM ports are defined, and can be augmented locally.

:ref:`aimk <aimk>` determines the machine architecture and executes
:command:`make`, passing it the architecture and a configuration file
along with arguments supplied to :ref:`aimk <aimk>`. It runs
:command:`make` in a subdirectory to prevent executables from becoming
intermixed and to permit overlapping compiles. A different
``makefile`` can be placed in each subdirectory or a single
``makefile``, :file:`Makefile.aimk`, can be shared between
architectures.  Per-architecture definitions from the
:file:`$PVM_ROOT/conf` directory are appended to the common
makefile. :ref:`Aimk <aimk>` calls command:`make` is called in one of
three ways, depending on what makefiles are present:

#. If :file:`$PVM_ARCH/Makefile` or :file:`$PVM_ARCH/makefile` exists,
   change directory to :envvar:`PVM_ARCH` and execute :command:`make`
   there:

   .. code-block:: csh

      (cd $PVM_ARCH ; make PVM_ARCH=$PVM_ARCH < aimk args >)

#. Else if :file:`Makefile.aimk` exists, create :envvar:`PVM_ARCH`
   directory if it doesn't exist, then:

   .. code-block:: csh

      (cd $PVM_ARCH ; \
      make -f $PVM_ROOT/conf/$PVM_ARCH.def \
      -f ../Makefile.aimk PVM_ARCH=$PVM_ARCH < aimk args >)

#. Else just execute :command:`make` in current directory:

   .. code-block:: csh

      make PVM_ARCH=$PVM_ARCH < aimk args >

If :ref:`aimk <aimk>` succeeds in calling :command:`make`, the exit
status is that of :command:`make`, otherwise it is ``1``.

Flags
-----

.. option:: -here

   Forces :ref:`aimk <aimk>` to run :command:`make` in the current
   directory, e.g., converts case 1. to case 3.

Examples
--------

The following :file:`Makefile.aimk` file builds and installs
:program:`hello`, creating the PVM binary directory if it doesn't
exist.  It can be run concurrently on machines of different types,
sharing the same source directory::

  LDIR    =  -L$(PVM_ROOT)/lib/$(PVM_ARCH)
  PVMLIB  =  -lpvm3
  SDIR    =  ..
  BDIR    =  $(HOME)/pvm3/bin
  XDIR    =  $(BDIR)/$(PVM_ARCH)
  CFLAGS  =  -g -I$(PVM_ROOT)/include
  LIBS    =  $(LDIR) $(PVMLIB) $(ARCHLIB)

  $(XDIR):
          - mkdir $(BDIR) $(XDIR)

  hello: $(SDIR)/hello.c $(XDIR)
          $(CC) $(CFLAGS) -o $@ $(SDIR)/$@.c $(LIBS)
          mv $@ $(XDIR)

Environment
-----------

:envvar:`PVM_ROOT`
   Root path of PVM installation.

:envvar:`PVM_ARCH`
   PVM architecture name for machine.

Files
-----

* :file:`$PVM_ROOT/lib/aimk`: The :ref:`aimk <aimk>` program
* :file:`$PVM_ROOT/conf/$PVM_ARCH.def`: Arch config file

See Also
--------

:ref:`pvm-intro`
