wxprot.conf manual page
=======================

Description
-----------
This man page describes the format of S.A.R.A.'s WX Protections configuration
files. See :manpage:`sara(7)` for an overview of S.A.R.A. or
:manpage:`saractl(8)` for an overview of the program used to manage
S.A.R.A's configuration.

Format
------
The configuration format is line oriented. Comments starts with *#*,
inline comments are supported. Every line is made up of two parts
seperated by a whitespace. The first part is the file path,
in case it contains a whitespace itself, the string can be enclosed in
double quotes or escaped. The path can be terminated with a '*' to make
it match every path that starts with the chosen prefix.
The second part is the flags list. It's a case in-sensitive and comma
separated list of the flags that need to be enabled. It can include
whitespaces before or after the commas.
Files in the */etc/sara/wxprot.conf.d/* directory are read in lexycografical
order and merged together at the end of */etc/sara/wxprot.conf* as if
they were a single big file.
In general, lines order doesn't matter, the rule with the most specific
path has precedence. In case of multiple entries with *exactly* the same
path, the first one has precedence and others are discarded.

Flags
^^^^^
WXORX
   Prevents any page of memory from being marked as
   both writable and executable at the same time.
STACK
   Prevents any page of memory in the stack from
   becoming executable if it could have been
   written in the past. (Depends on WXORX)
HEAP
   Prevents any page of memory in the heap from
   becoming executable if it could have been
   written in the past. (Depends on WXORX)
OTHER
   Prevents any other page of memory from becoming
   executable if it could have been written in
   the past. (Depends on WXORX)
MPROTECT
   Enables WXORX, STACK, HEAP and OTHER
MMAP
   Prevents new executable mmap after
   the dynamic libraries have been loaded.
   (Depends on OTHER)
FULL
   Enables MPROTECT and MMAP.
EMUTRAMP
   Enables trampoline emulation, if trampoline
   emulation is missing, it's changed to whatever
   is set in "wxprot_emutramp_missing_default".
   (Depends on MPROTECT and conflicts with any
   other EMUTRAMP*)
EMUTRAMP_OR_MPROTECT
   Like EMUTRAMP but, if trampoline emulation 
   is missing, it's changed to MPROTECT.
   (Depends on MPROTECT and conflicts with any
   other EMUTRAMP*)
EMUTRAMP_OR_NONE
   Like EMUTRAMP but, if trampoline emulation
   is missing, all the flags are replaced with
   NONE. (Depends on MPROTECT and conflicts with
   any other EMUTRAMP*)
VERBOSE
   Verbosely report every violation. (Depends on
   WXORX)
COMPLAIN
   Don't actually block anything. If VERBOSE
   is enabled too S.A.R.A will reports violations.
   (Depends on WXORX)
TRANSFER
   Child tasks will inherit this task's flags
   despite what is written in the configuration.
NONE
   Disables everything.

Examples
--------
Enable full reporting, without enforcement, to any executable under /bin/::

	/bin/* FULL,COMPLAIN,VERBOSE

Enable MPROTECT with verbose reporting on everything::

	* MPROTECT,VERBOSE

See also
--------

:manpage:`sara(7)`, :manpage:`saractl(8)`, :manpage:`sara-xattr(8)`
and <https://sara.smeso.it>
