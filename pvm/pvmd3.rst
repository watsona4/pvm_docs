.. _pvmd3:

PVM Daemon
==========

.. program:: pvmd

:ref:`pvmd3 <pvmd3>` is a daemon process which coordinates UNIX hosts
in a virtual machine. One :ref:`pvmd3 <pvmd3>` must run on each host
in the group. They provide the communication and process control
functions needed by the user's PVM processes. The daemon can be
started manually with a host file argument that will automatically
start the remote :ref:`pvmd <pvmd3>`\ s. The local and remote
:ref:`pvmd <pvmd3>`\ s can also be started from the PVM console program
:ref:`pvm <pvm>`.

The name of the daemon executable is :ref:`pvmd3 <pvmd3>`. It is
usually started by a shell script, :file:`$PVM_ROOT/lib/pvmd`.

.. versionchanged:: 3.4
   Before running :ref:`pvmd3 <pvmd3>`, :ref:`pvmd <pvmd3>` sources any
   commands in :file:`$HOME/.pvmprofile` if this file exists.

The following options may be specified on the command line when
starting the master :ref:`pvmd <pvmd3>` or PVM console:

.. option:: -d mask

   Set :ref:`pvmd <pvmd3>` debug mask.  Used to debug the :ref:`pvmd
   <pvmd3>` or :ref:`libpvm <libpvm>` (not intended to be used to
   debug application programs). Mask is the sum of the following bits
   and can be specified in hexadecimal (``0x...``), octal (``0...``)
   or decimal:
   
   ========== ================================================
   Bit        Information
   ========== ================================================
   ``0x1``    Packet routing
   ``0x2``    Message routing and entry points
   ``0x4``    Task state
   ``0x8``    Slave :ref:`pvmd <pvmd3>` startup
   ``0x10``   Host table updates
   ``0x20``   Select loop (below packet layer)
   ``0x40``   IP network
   ``0x80``   Multiprocessor nodes
   ``0x100``  Resource manager interface
   ``0x200``  Application (messages with no destination, etc.)
   ``0x400``  Wait contexts
   ``0x800``  Shared memory operations
   ``0x1000`` Semaphores
   ``0x2000`` Locks
   ``0x4000`` Message route control
   ========== ================================================

.. option:: -n name

   Specify an alternate hostname for the master :ref:`pvmd <pvmd3>` to
   use. Useful when :func:`gethostname` returns a name not assigned to
   any network interface.

The following options are used by the master :ref:`pvmd <pvmd3>` when
starting slaves and are only of interest to someone writing a hoster.
Don't just go using them, now.

.. option:: -s

   Start :ref:`pvmd <pvmd3>` in slave mode. Hostfile cannot be used
   and five additional parameters must be supplied: master :ref:`pvmd
   <pvmd3>` index, master IP, master MTU, slave :ref:`pvmd <pvmd3>`
   index, and slave IP.

.. option:: -S

   Same as :option:`pvmd3 -s`, but slave :ref:`pvmd <pvmd3>` doesn't
   wait for its ``stdin`` to be closed after printing its parameters.
   Used for manual startup.

.. option:: -f

   Slave doesn't fork after configuration (useful if the slave is to
   be controlled or monitored by some process).

Host File Format
----------------

Each host in the virtual machine must have an entry in the host file.
Lines beginning with a splat (``#``), optionally preceded by whitespace,
are ignored.

A simple host file might look like::

   # my first host file
   thud
   fred
   wilma
   barney
   betty

This specifies the names of five hosts to be configured in the virtual
machine.

The master :ref:`pvmd <pvmd3>` for a group is started by hand on the
localhost, and it starts slaves on each of the remaining hosts using
the :command:`rsh` or :command:`rexec` command. The master host may
appear on any line of the host file. Host names cannot be numeric (IP)
addresses, because they are passed to :command:`rsh` and
:command:`rexec`, which usually don't accept addresses.

The simple format above works fine if you have the same login name on
all five machines and the name of the master host in your
:file:`.rhosts` files on the other four.

There are several host file options available:

``lo=NAME``
   Specifies an alternate login name (`NAME`) to use.

``so=pw``
   This is necessary when the remote host cannot trust the master.
   Causes the master :ref:`pvmd <pvmd3>` to prompt for a password for
   the remote host in the ``tty`` of the :ref:`pvmd <pvmd3>` (note you
   can't start the master using the console or background it when
   using this option) you will see::

      Password (honk.cs.utk.edu:manchek):

   you should type your password for the remote host. The startup
   will then continue as normal.

``dx=FILE``
   Specifies the path of the :ref:`pvmd <pvmd3>` executable. `FILE`
   may be a simple filename, an absolute pathname, or a path relative
   to the user's home directory on the remote host. This is mainly
   useful to aid in debugging new versions of PVM, but may have other
   uses.

``ep=PATH``
   Specifies a path for the :ref:`pvmd <pvmd3>` to search for
   executable program components when spawning a new process. The path
   may have multiple elements, separated by colons (``:``).

``wd=PATH``
   Specifies a working directory in which all spawned tasks on this
   host will execute.

``sp=VALUE``
   Specifies the relative computational speed of this host compared to
   other hosts in the configuration. `VALUE` is an integer in the range
   [1--1000000]

``bx=PATH``
   Specifies the debugger program path.  Note: the environment
   variable :envvar:`PVM_DEBUGGER` can also be set.

``ip=NAME``
   Specifies an alternate IP address to use for the host. As with host
   names (when :option:`ip=` is not used), the address must be a host
   name, not a numeric address, because it is passed to :command:`rsh`
   and :command:`rexec`. This option allows one to pick a specific
   network interface for a machine without using the interface's name.
   It can also be used to create a virtual machine using symbolic
   (instead of actual) host names.

``so=ms``
   Rarely used. Causes the master :ref:`pvmd <pvmd3>` to request user
   to manually perform the startup of a :ref:`pvmd <pvmd3>` on a slave
   host when :command:`rsh` and :command:`rexec` network services are
   disabled but IP connectivity exists. See section
   :ref:`pvmd3-manual-startup`.

``id=VMID``
   .. versionadded:: 3.4.4
      A new feature in PVM 3.4.4 is the concept of a "Virtual Machine
      ID". You can now set the `VMID` to an arbitrary string and this
      will distinguish and allow multiple virtual machines to run on
      the same set of hosts under the same ``userid``. (This feature
      was originally introduced by SGI in their commercial PVM
      product, and has now been generalized for the public PVM
      system.)  This feature seems to be something that people often
      want, and the :option:`id=` hostfile option (or
      :envvar:`PVM_VMID` environment variable) is the cleanest way to
      provide this functionality, rather than overloading the
      :macro:`SHAREDTMP` compiler flag and other internals.

      .. note::
	 Make sure that you appropriately set the :envvar:`PVM_VMID`
	 environment variable in any shells from which PVM application
	 tasks or the :ref:`pvm <pvm>` console will be run, or else
	 they won't know which virtual machine to attach to!

      By default, all hosts which are added to the virtual machine
      will inherit the same VMID. If hosts are added to the virtual
      machine which are running older versions of PVM (prior to
      3.4.4), then the VMID will be ignored for those hosts, and hence
      these machines can only be added to one virtual machine for the
      given user. The VMID need not be consistent on every host in a
      virtual machine (although this is not necessarily advisable).

A dollar sign (``$``) in an option introduces a variable name, for
example :envvar:`PVM_ARCH`. Names are expanded from environment
variables by each :ref:`pvmd <pvmd3>`.

Each of the flags above has a default value. These are:

* ``lo``: The loginname on the master host.
* ``so``: Nothing
* ``dx``: :file:`$PVM_ROOT/lib/pvmd` (or environment variable
  :envvar:`PVM_DPATH`)
* ``ep``: :file:`$HOME/pvm3/bin/$PVM_ARCH:$PVM_ROOT/bin/$PVM_ARCH`
* ``wd``: :file:`$HOME`
* ``sp``: 1000
* ``bx``: :file:`$PVM_ROOT/lib/debugger`

You can change these by adding a line with a star (``*``) in the first
field followed by the options, for example::

  * lo=afriend so=pw

This sets new default values for ``lo`` and ``so`` for the remainder
of the host file, or until the next ``*`` line. Options set on the
last ``*`` line also apply to hosts added dynamically using
:func:`pvm_addhosts`.

Host options can be set without starting the hosts automatically.
Information on host file lines beginning with ``&`` is stored, but the
hosts are not started until added using :func:`pvm_addhosts`.

Example host file::

  # host file for testing on various platforms
  fonebone
  refuge
  # installed in /usr/local here
  sigi.cs dx=/usr/local/pvm3/lib/pvmd
  # borrowed accts, "guest", don't trust fonebone
  * lo=guest  so=pw
  sn666.jrandom.com
  cubie.misc.edu
  # really painful one, must start it by hand and share a homedir
  & igor.firewall.com  lo=guest2  so=ms  ep=bob/pvm3/bin/$PVM_ARCH

.. _pvmd3-manual-startup:

Manual Startup
--------------

When adding a host with this option set you will see on the ``tty`` of
the :ref:`pvmd <pvmd3>`::

  *** Manual startup ***
  Login to "honk" and type:
  $PVM_ROOT/lib/pvmd -S -d0 -nhonk 1 80a9ca95:0cb6 4096 2 80a95c43:0000
  Type response:

after typing the given command on host ``honk``, you should see a line
like::
      
  ddpro<2312> arch<ALPHA> ip<80a95c43:0a8e> mtu<4096>

type this line on the ``tty`` of the master :ref:`pvmd <pvmd3>`. You
should then see::
           
  Thanks

and the two :ref:`pvmd <pvmd3>`\ s shoulqd be able to communicate.

Note you can't start the master using the console or background it when
using this option.

Overloading Hosts
-----------------

You can force PVM to overload a host (start more than one :ref:`pvmd
<pvmd3>` on it) by putting a ``$`` before the host name in the host
file. This is not recommended unless you know what you're doing and
have a good reason for it. You must build the PVM source with option
:macro:`OVERLOADHOST` defined for it to work.

You may also need to use the :option:`ip=` hostfile option to define
several names with the same IP address. If two or more hosts in a PVM
have the same name, they cannot be identified uniquely.

Stopping :ref:`pvmd3 <pvmd3>`
-----------------------------

The preferred method of stopping all the :ref:`pvmd <pvmd3>`\ s is to
give the halt command in the PVM console. This kills all PVM tasks,
all the remote daemons, the local daemon, and finally the console
itself. If the master :ref:`pvmd <pvmd3>` is killed manually it should
be sent a ``SIGTERM`` signal to allow it to kill the remote :ref:`pvmd
<pvmd3>`\ s and clean up various files.

The :ref:`pvmd <pvmd3>` can be killed in a manner that leaves the file
:file:`/tmp/pvmd.uid` behind on one or more hosts. ``uid`` is the
numeric user ID (from :file:`/etc/passwd`) of the user. This will
prevent PVM from restarting on that host. Deletion of this file will
fix this problem:

.. code-block:: csh

   rm `( grep $user /etc/passwd || ypmatch $user passwd ) | awk -F:
   '{print "/tmp/pvmd."$3; exit}'`

Files
-----

* :file:`$PVM_ROOT/lib/pvmd`: PVM daemon startup script
* :file:`$PVM_ROOT/lib/$PVM_ARCH/pvmd3`: PVM daemon executable
* :file:`$HOME/.pvmprofile`: Shell commands read by :ref:`pvmd
  <pvmd3>` before running :ref:`pvmd3 <pvmd3>`
* :file:`$HOME/pvm3/bin/$PVM_ARCH`" Private PVM executable directory
* :file:`$PVM_ROOT/pvm3/bin/$PVM_ARCH`: System PVM executable
  directory
* :file:`/tmp/pvmd.uid`: :ref:`pvmd <pvmd3>` local socket address
* :file:`/tmp/pvml.uid`: :ref:`pvmd <pvmd3>` runtime error log
* :file:`$HOME/.rhosts`: File allowing access to a host from other hosts

See Also
--------
:ref:`pvm <pvm>`, :ref:`pvm-intro`, :command:`rhosts`
