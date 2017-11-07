sara-xattr manual page
======================

Synopsys
--------
**sara-xattr** [*options*] <*command*> <*filename*> [*flags*]

Description
-----------
:program:`sara-xattr` is the userspace utility that manages S.A.R.A. LSM's
extended attributes.
The optional *flags* field is used only with the `set` command.
Flags format is the same of :manpage:`wxprot.conf(5)`'s config files.

Commands
--------

set
   Set specified xattr on a file.
get
   get xattr from a file
del
   delete xattr from a file

Options
-------
  -h, --help            show this help message and exit
  -v, --verbose         Be verbose. For extra verbosity use multiple -v.
  -q, --quiet           Suppress any output.
  -V, --version         show program's version number and exit
  -c CONFIG_DIR, --config-dir CONFIG_DIR
                        Specify config directory. Defaults to "/etc/sara"
  -S SECURITYFS, --securityfs SECURITYFS
                        The mount point of the securityfs. Defaults to
                        "/sys/kernel/security".
  -s {wxprot}, --submodule {wxprot}
                        Select the submodule you want to work with. Available
                        submodules: wxprot.
  -u, --user            Use user xattr namespace instead of the security
                        namespace.
  -b, --both            Use both user and security xattr namespace.

Examples
--------
Turn off WX Protections for an executable:

.. code-block:: bash

	sara-xattr -s wxprot set /usr/bin/program none

Turn on all WX Protections for an executable:

.. code-block:: bash

	sara-xattr -s wxprot set /usr/bin/program full,verbose

Bugs
----
If you find any bugs, please report them at
<https://github.com/smeso/saractl/issues>

See also
--------

:manpage:`sara(7)`, :manpage:`wxprot.conf(5)`, :manpage:`saractl(8)`,
:manpage:`xattr(7)` and <https://sara.smeso.it>
