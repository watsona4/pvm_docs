.. _pvm-intro:

###############################
Parallel Virtual Machine System
###############################
       
:abbr:`PVM (Parallel Virtual Machine)` is a software system that
enables a collection of heterogeneous computers to be used as a
coherent and flexible concurrent computational resource.

The individual computers may be shared- or local-memory
multiprocessors, vector supercomputers, specialized graphics engines,
or scalar workstations, that may be interconnected by a variety of
networks, such as ethernet, FDDI.

User programs written in C, C++ or Fortran access PVM through library
routines (:ref:`libpvm3.a <libpvm>` and :ref:`libfpvm3.a <libpvm>`).

Daemon programs (:ref:`pvmd3 <pvmd3>`) provide communication and
process control between computers.

Contents
========

.. toctree::
   :maxdepth: 2

   user_commands
   library_functions

Machine Architecture
====================

In the PVM system, machines are assigned a short string to identify
their architectures (this includes operating system type as well as
CPU type). The types currently predefined in the distribution are:

============= ====================================================
Architecture  Description
============= ====================================================
AFX8          Alliant FX/8
ALPHA         DEC Alpha/OSF-1
ALPHAMP       DEC Alpha/OSF-1 / using shared memory
AIX46K        IBM/RS6000 / AIX 4.x
AIX4MP        IBM SMP / shared memory transport / AIX 4.x
AIX4SP2       IBM SP-2 / using MPI / AIX 4.x
APOLLO        HP 300 running Domain/OS
ATT           AT&T/NCR 3600 running SysVR4
BAL           Sequent Balance
BFLY          BBN Butterfly TC2000
BSD386        80[345]86 running BSDI or BSD386
CM2           Thinking Machines CM-2 Sun front-end
CM5           Thinking Machines CM-5
CNVX          Convex using IEEE floating-point
CNVXN         Convex using native f.p.
CRAY          Cray
CRAY2         Cray-2
CRAYSMP       Cray S-MP
CSPP          Convex Exemplar
CYGWIN        POSIX emulation layer on top of Windows32
DGAV,DGIX     Data General Aviion
E88K          Encore 88000
FREEBSD       80[345]86 running FreeBSD
HP300         HP 9000 68000 cpu
HPPA          HP 9000 PA-Risc
HPPAMP        HP 9000 PA-Risc / shared memory transport
KSR1          Kendall Square
I860          Intel RX Hypercube
IPSC2         Intel IPSC/2
LINUX         80[345]86 running Linux
LINUXALPHA    DEC Alpha running Linux
LINUXARM      Strogarm running Linux
LINUXHPPA     HP 9000 running Linux
LINUXPPC      PowerPC running Linux
LINUXSPARC    Sparc running Linux
M88K          Motorola M88100 running Real/IX
M88K          Motorola M88100 running Real/IX
MASPAR        Maspar
MIPS          Mips
NETBSDALPHA   DEC Alpha running NetBSD
NETBSDAMIGA   Amiga running NetBSD
NETBSDARM32   Strongarm running NetBSD
NETBSDHP300   HP 300 running NetBSD
NETBSDI386    80[345]86 running NetBSD
NETBSDM68K    Any Motorola 68K running NetBSD
NETBSDMAC68K  Macintosh running NetBSD
NETBSDMIPSEB  Mips EB running NetBSD
NETBSDMIPSEL  Mips EL running NetBSD
NETBSDNS32K   NS32K running NetBSD
NETBSDPMAX    DEC Pmax running NetBSD
NETBSDPOWERPC PowerPC running NetBSD
NETBSDSH3     SH3 running NetBSD
NETBSDSPARC   Sparc running NetBSD
NETBSDSPARC64 Sparc64 running NetBSD
NETBSDSUN3    SUN 3 running NetBSD
NETBSDVAX     Vax running NetBSD
NEXT          NeXT
OS2           OS/2
PGON          Intel Paragon
PMAX          DEC/Mips arch (3100, 5000, etc.)
RS6K          IBM/RS6000 / AIX 3.x
RS6KMP        IBM SMP / shared memory transport / AIX 3.x
RT            IBM/RT
SCO           80[345]86 running SCO Unix
SGI           Silicon Graphics IRIS
SGI5          Silicon Graphics IRIS running OS 5.0
SGI6          Silicon Graphics IRIS running OS >= 6.0
SGI64         Silicon Graphics IRIS running 64 bit
SGIMP         Silicon Graphics IRIS / OS 5.x / using shared memory
SGIMP6        Silicon Graphics IRIS / OS 6.x / using shared memory
SGIMP64       Silicon Graphics IRIS / 64 bit / using shared memory
SP2MPI        IBM SP-2 / using MPI / AIX 3.x
SUN3          Sun 3
SUN4          Sun 4, 4c, sparc, etc.
SUN4SOL2      Sun 4 running Solaris 2.x
SUNMP         Sun 4 / using shared memory / Solaris 2.x
SX3           NEC SX-3
SYMM          Sequent Symmetry
TITN          Stardent Titan
U370          IBM 3090 running AIX
UTS2          Amdahl running UTS
UVAX          DEC/Microvax
UWARE         Uware
UXPM          Fujitsu running UXP/M
VCM2          Thinking Machines CM-2 Vax front-end
WIN32         Windows 95/98/NT
X86SOL2       80[345]86 running Solaris 2.x
============= ====================================================

Environment Variables
=====================

The following environment variables are read by PVM and may be set in
order to customize your PVM environment. To set them, you can add
commands to your :file:`.cshrc` or :file:`.profile` or equivalent
shell startup file. See the manual page for the shell you normally
use for information about how to do this. You can also include an
appropriate shell startup file stub to set PVM environment variables
and to add PVM directories to your execution path. Inert the matching
stub file, :file:`pvm3/lib/cshrc.stub`, :file:`pvm3/lib/kshrc.stub` or
:file:`pvm3/lib/bashrc.stub`, after your declaration of
:envvar:`PVM_ROOT` in your shell startup file.

For csh users: Note that setting them in :file:`.login` does not have
the same effect. The :file:`.login` script file is only read when you
are actually logging in, whereas :file:`.cshrc` is read every time
:command:`csh` starts up. PVM needs to have environment variables set
when it starts a slave :ref:`pvmd <pvmd3>` with ``rsh host pvmd ...``,
so they must be set in :file:`.cshrc`.

For those using a shell that doesn't always read a startup script
(e.g. :command:`sh`, :command:`ksh`), there is another way to set
environment variables for PVM. Before running the PVM executables, the
:ref:`pvm <pvm>` and :ref:`pvmd <pvmd3>` startup scripts source any
commands in :file:`$HOME/.pvmprofile` if this file exists.

The following environment variables are supported by PVM 3.4.4:

.. envvar:: PVM_ROOT

   The path where PVM libraries and system programs are installed, for
   example :file:`/usr/local/pvm3` or :file:`$HOME/pvm3`. This
   variable must be set on each host where PVM is used in order for
   PVM to function. There is no default value.

.. envvar:: PVM_TMP

   The path for PVM temporary files, such as the daemon socket file
   :file:`pvmd.<uid>` and the log file :file:`pvml.<uid>`. Use this
   environment variable to use a directory other than :file:`/tmp` (or
   :file:`C:\TEMP` on Win32), or to introduce added security by using
   a protected subdirectory in :file:`/tmp` that is owned by your
   ``userid`` and cannot be easily corrupted.

.. envvar:: PVM_RSH

   The path to the :command:`rsh` program on your system, if different
   than that defined in the :file:`$PVM_ROOT/conf/$PVM_ARCH.def`
   configuration file. This environment variable can also be used to
   replace :command:`rsh` with :command:`ssh` for added security.

.. envvar:: PVM_PATH

   The execution path to be searched for PVM programs on your
   system. By default, PVM looks in :file:`$HOME/pvm3/bin/$PVM_ARCH`
   and :file:`$PVM_ROOT/bin/$PVM_ARCH` for your PVM applications. This
   environment variable does not override the :option:`ep=` host file
   option.

.. envvar:: PVM_WD

   The working directory for spawned PVM programs on your system. By
   default, PVM spawns your PVM applications in :file:`$HOME`, but for
   convenience in accessing data or input files using relative path
   names, an alternate working directory can be specified. This
   environment variable does not override the :option:`wd=` host file
   option.

.. envvar:: PVM_EXPORT

   Names of environment variables to export from a parent task to
   children tasks through :func:`pvm_spawn`. Multiple names must be
   separated by ``:``. If :envvar:`PVM_EXPORT` is not set, no
   environment is exported.

.. envvar:: PVM_DEBUGGER

   The debugger script to use when :func:`pvm_spawn` is called with
   :macro:`PvmTaskDebug` set. The default is
   :file:`$PVM_ROOT/lib/debugger`.

.. envvar:: PVM_DPATH

   The path of the :ref:`pvmd <pvmd3>` startup script (default is
   :file:`$PVM_ROOT/lib/pvmd`). It is overridden by host file option
   :option:`dx=`. This variable is useful if you use a shell that
   doesn't automatically execute a startup script (such as
   :file:`.cshrc`) to allow setting :envvar:`PVM_ROOT` on slave
   (added) hosts. If you set it to the absolute or relative path of
   the :ref:`pvmd <pvmd3>` startup script (for example
   :file:`/usr/local/pvm3/lib/pvmd` or :file:`pvm3/lib/pvmd`), the
   script will automatically set :envvar:`PVM_ROOT`. Note that for
   this to work, you must set it to run the :ref:`pvmd <pvmd3>` script,
   not the :ref:`pvmd3 <pvmd3>` executable itself.

.. envvar:: PVM_WINDPATH

   This variable serves the same purpose as the :envvar:`PVM_DPATH`
   above, but specifically for Win32 systems. This second environment
   variable allows for alternate specification of the path to the
   :ref:`pvmd3.exe <pvmd3>` daemon executable using appropriate DOS
   file path syntax and environment variables (e.g.
   :file:`%PVM_ROOT%\lib\WIN32\pvmd3.exe`).

.. envvar:: PVMHOSTFILE

   Specifies the path to an optional host file to be used by default
   when starting PVM. This alleviates the need to manually pass a host
   file path argument to the :ref:`pvm <pvm>` console or :ref:`pvmd
   <pvmd3>` script when starting PVM.

.. envvar:: PVMDLOGMAX

   Sets the maximum length of the :ref:`pvmd <pvmd3>` error log
   file. Default value is the :envvar:`PVMDLOGMAX` parameter in the
   source, ``1 Mbyte``.

.. envvar:: PVMDDEBUG

   Sets the default :ref:`pvmd <pvmd3>` debugging mask (as does the
   :option:`pvmd -d` option).  Value can be in hexadecimal
   (``0x...``), octal (``0...``) or decimal.  Used to debug the
   :ref:`pvmd <pvmd3>` (not intended to be used to debug application
   programs).

.. envvar:: PVMTASKDEBUG

   Sets the default :ref:`libpvm` debugging mask (as does
   :func:`pvm_setopt(PvmDebugMask, x) <pvm_setopt>`). Value can be in
   hexadecimal (``0x...``), octal (``0...``) or decimal. Used to debug
   :ref:`libpvm` (not intended to be used to debug application
   programs).

.. envvar:: PVMTASK

   Sets additional flag bits for the :func:`pvm_spawn` library
   call. Allows override at run time of flags compiled into the
   :func:`pvm_spawn` calls in PVM application, e.g. to turn on
   :macro:`PvmTaskDebug` for popping up child tasks in a debugger
   window.

.. envvar:: PVMBUFSIZE

   Sets the size of the shared memory buffers used by libpvm and the
   :ref:`pvmd <pvmd3>`. The default value is 1,048,576. If your program
   composes messages longer than this size, you must increase it.

.. envvar:: PVM_VMID

   A new feature in PVM 3.4.4 is the concept of a "Virtual Machine
   ID". You can now set the :envvar:`PVM_VMID` environment variable to
   an arbitrary string (or use the :option:`id=` option in a host
   file, see page for :ref:`pvmd3 <pvmd3>`), and this will distinguish
   and allow multiple virtual machines to run on the same set of hosts
   under the same ``userid``. (This feature was originally introduced
   by SGI in their commercial PVM product, and has now been
   generalized for the public PVM system.) This feature seems to be
   something that people often want, and the :envvar:`PVM_VMID` is the
   cleanest way to provide this functionality, rather than overloading
   the :macro:`SHAREDTMP` compiler flag and other internals.

   Setting the :envvar:`PVM_VMID` environment variable before starting
   PVM will create an encapsulated virtual machine with the given VMID
   name. By default, all other hosts which are added to this virtual
   machine will inherit the same VMID. If hosts are added to the
   virtual machine which are running older versions of PVM (prior to
   3.4.4), then the VMID will be ignored for those hosts, and hence
   these machines can only be added to one virtual machine for the
   given user. The VMID need not be consistent on every host in a
   virtual machine (although this is not necessarily advisable), and
   the VMID can be set for individual hosts using the :option:`id=`
   host file option (see page for :ref:`pvmd3 <pvmd3>`).

The following environment variables are used by PVM internally. With
the exception of :envvar:`PVM_ARCH`, their values should not be
modified. This is for information only.

.. envvar:: PVM_ARCH

   The PVM architecture name of the host on which it is set, used to
   distinguish between machines with different executable (``a.out``)
   formats. Copies of a program for different architectures are
   installed in parallel directories named for PVM architectures.

.. envvar:: PVMSOCK

   Is passed from :ref:`pvmd <pvmd3>` to spawned task, and gives the
   address of the :ref:`pvmd <pvmd3>` local socket.

.. envvar:: PVMEPID

   Holds the expected process ID of a spawned task ``exec``\ 'd by the
   :ref:`pvmd <pvmd3>`.  This is a magic cookie used by the task to
   identify itself when reconnecting to the :ref:`pvmd <pvmd3>`, in
   order to get the correct task slot.

.. envvar:: PVMTMASK

   The :ref:`libpvm` trace mask, passed from the :ref:`pvmd <pvmd3>` to
   spawned tasks.

.. envvar:: PVMTRCBUF

   The :ref:`libpvm` trace buffer size. If specified determines the
   number of bytes of trace event message buffer to be collected
   before sending to front-end tracer program.

.. envvar:: PVMTRCOPT

   The :ref:`libpvm` trace option setting. Determines the level of
   tracing to be performed on invocations of PVM library calls.

.. envvar:: PVMINPLACEDELAY

   Used to optimize sending of :macro:`PvmDataInPlace` messages on MPP
   systems.

.. envvar:: PVMKEY

   PVM uses this value, combined with the process ID, to generate
   shared-memory segment keys. The default value is your numeric
   ``uid``.  PVM automatically detects collisions when generating a
   key and picks a new key, so it should almost never need to be set
   explicitly.

See Also
========

:ref:`aimk <aimk>`, :ref:`pvm <pvm>`, :ref:`pvmd3 <pvmd3>`,
:download:`PVM 3.3 User's Guide and Reference Manual
<_static/pvm_guide.pdf>`

Authors
=======

* A.L. Beguelin (Carnegie Mellon University and Pittsburgh
  Supercomputer Center, Pittsburgh PA)
* J.J. Dongarra (University of Tennessee, Knoxville TN, and Oak Ridge
  National Laboratory, Oak Ridge TN)
* G.A. Geist (Oak Ridge National Laboratory, Oak Ridge TN)
* W.C. Jiang (University of Tennessee, Knoxville TN)
* R.J. Manchek (University of Tennessee, Knoxville TN)
* B.K. Moore (University of Tennessee, Knoxville TN)
* V.S. Sunderam (Emory University, Atlanta GA)

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

