.. _pvm:

PVM Version 3 Console
=====================

.. program:: pvm

:ref:`pvm <pvm>` is a stand alone PVM task which allows the user to
interactively query and modify the virtual machine. The console can be
started and stopped multiple times on any of the hosts in the virtual
machine without affecting PVM or any applications that may be running.

When started pvm determines if PVM is already running and if not
automatically executes :ref:`pvmd3 <pvmd3>` on this host, passing
:ref:`pvmd3 <pvmd3>` the command line options and host file. Thus PVM
need not be running to start the console. Once started the console
prints the prompt: ``pvm>``.

The following console commands are available:

.. option:: add hostname(s)

   Add hosts to virtual machine.
   
.. option:: alias

   Define/list command aliases.

.. option:: conf

   List virtual machine configuration.

.. option:: delete hostname(s)

   Delete hosts from virtual machine.

.. option:: echo

   Echo arguments.

.. option:: export

   Add environment variables to spawn export list.

.. option:: halt

   Stop :ref:`pvmd <pvmd3>`\ s.

.. option:: help [command]

   Print helpful information about a command.

.. option:: id

   Print console task ID.

.. option:: jobs

   List running jobs.

.. option:: kill task-tid

   Terminate tasks.

.. option:: mstat host-tid

   Show status of hosts.

.. option:: ps -a

   List all PVM tasks.

.. option:: pstat task-tid

   Show status of tasks.

.. option:: quit

   Exit console.

.. option:: reset

   Kill all tasks.

.. option:: setenv

   Display/set environment variables.

.. option:: sig signum task

   Send signal to task.

.. option:: spawn [opt] a.out

   Spawn task.

.. option:: trace

   Set/display trace event mask.

.. option:: unexport

   Remove environment variables from spawn export list.

.. option:: unalias

   Undefine command alias.

.. option:: version

   Show :ref:`libpvm` version.

The options to :option:`pvm spawn` are:

.. program:: pvm spawn

.. option:: -count

   Number of tasks `count`, default is 1.

.. option:: -host

   Spawn on `host`, default is any.

.. option:: -ARCH

   Spawn on hosts of `ARCH`.

.. option:: -?

   Enable debugging.

.. option:: ->

   Redirect task output to console.

.. option:: ->file

   Redirect task output to `file`.

.. option:: ->>file

   Redirect task output append to `file`.

:ref:`pvm <pvm>` reads :file:`$HOME/.pvmrc` before reading commands
from the ``tty``, so it can be used to customize the console
environment, for example:

.. code-block:: csh

   pvm> alias ? help
   pvm> alias j jobs
   pvm> setenv PVM_EXPORT DISPLAY
   pvm> # print my id
   pvm> echo new pvm shell
   pvm> id

Examples
--------

.. code-block:: bash

   $ pvm

Starts up :ref:`pvmd3 <pvmd3>` on the local host or connects to
running :ref:`pvmd3 <pvmd3>`.

.. code-block:: bash

   $ pvm hostfile
       
Starts up console and :ref:`pvmd3 <pvmd3>`, which inturn reads the
host file and adds the listed computers to the virtual machine.

See Also
--------

:ref:`pvm-intro`, :ref:`pvmd3 <pvmd3>`
