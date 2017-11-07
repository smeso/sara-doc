saractl manual page
===================

Synopsys
--------
**saractl** [*options*] <*command*> [*command-specific options* ...]

Description
-----------
:program:`saractl` is the userspace utility that manages S.A.R.A. LSM's
configurations.

Commands
--------

load
   Load configurations. If a config is already present
   and up to date it won't be loaded again (-s is ignored).
startup
   Load configurations for the first time at boot (-s is
   ignored).
enable
   Enable S.A.R.A.
disable
   Disable S.A.R.A.
status
   Get S.A.R.A. status.
lock
   Prevent changing the config until next reboot.
config_to_file
   Generate various binary formats to import the
   configuration without saractl (-s is ignored).
test
   Run some self-tests.

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
  -s {main,wxprot}, --submodule {main,wxprot}
                        Select the submodule you want to work with. Available
                        submodules: wxprot. Defaults to "main".
  -f, --force           Force reload even if the config is already up to date
                        (to use only after the *load* command).
  -F {binary,sh,c}, --output-format {binary,sh,c}
                        Select the desired output format. Available formats:
                        "binary", "sh" and "c". Defaults to "binary"
                        (to use only after the *config_to_file* command).
  -o OUTPUT, --output OUTPUT
                        Output file or directory. Defaults to "./output/"
                        directory for "binary" format, "./output.sh" file for
                        "sh" format and "./output.c" file for "c" format
                        (to use only after the *config_to_file* command).

Examples
--------
Reload the config:

.. code-block:: bash

	saractl load

Create a stand alone shell script to load the config:

.. code-block:: bash

	saractl config_to_file -F sh -o ./script.sh

Config file
-----------

The main configuration file for saractl can be found in */etc/sara/main.conf*.
These are the available options::

	sara_enabled=1				# enable S.A.R.A. LSM

	sara_locked=0				# lock S.A.R.A. config
						# when it has been loaded

	wxprot_enabled=1			# enable WX Protections

	wxprot_emutramp_missing_default=none	# default option to use
						# when emutramp is not
						# supported.
						# It can be set to "none"
						# or "mprotect".

	wxprot_xattr_enabled=0			# enable security XATTRs
						# support

	wxprot_xattr_user_allowed=0		# enable user XATTRs support

Bugs
----
If you find any bugs, please report them at
<https://github.com/smeso/saractl/issues>

See also
--------

:manpage:`sara(7)`, :manpage:`wxprot.conf(5)`, :manpage:`sara-xattr(8)`
and <https://sara.smeso.it>
