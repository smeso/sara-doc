============
S.A.R.A. LSM
============

Introduction
************

S.A.R.A. (S.A.R.A. is Another Recursive Acronym) is a stacked Linux Security
Module that aims to collect heterogeneous security measures, providing a common
interface to manage them.
As of today it consists of one submodule:

- WX Protection


The kernel-space part is complemented by its user-space counterpart: `saractl`
[1]_.
A test suite for WX Protection, called `sara-test` [3]_, is also available.
You can also visit the `official home page of S.A.R.A.` [4]_.

At the time of writing S.A.R.A. has been proposed for upstreaming, but it's
still out of tree.

-------------------------------------------------------------------------------

S.A.R.A.'s Submodules
=====================

WX Protection
-------------
WX Protection aims to improve user-space programs security by applying:

- `W^X enforcement`_
- `W!->X (once writable never executable) mprotect restriction`_
- `Executable MMAP prevention`_

All of the above features can be enabled or disabled both system wide
or on a per executable basis through the use of configuration files managed by
`saractl` [1]_.

It is important to note that some programs may have issues working with
WX Protection. In particular:

- **W^X enforcement** will cause problems to any programs that needs
  memory pages mapped both as writable and executable at the same time e.g.
  programs with executable stack markings in the *PT_GNU_STACK* segment.
- **W!->X mprotect restriction** will cause problems to any program that
  needs to generate executable code at run time or to modify executable
  pages e.g. programs with a *JIT* compiler built-in or linked against a
  *non-PIC* library.
- **Executable MMAP prevention** can work only with programs that have at least
  partial *RELRO* support. It's disabled automatically for programs that
  lack this feature. It will cause problems to any program that uses *dlopen*
  or tries to do an executable mmap. Unfortunately this feature is the one
  that could create most problems and should be enabled only after careful
  evaluation.

To extend the scope of the above features, despite the issues that they may
cause, they are complemented by `/proc/PID/attr/sara/wxprot interface`_
and `Trampoline emulation`_.
It's also possible to override the centralized configuration via `Extended
filesystem attributes`_.

At the moment, WX Protection (unless specified otherwise) should work on
any architecture supporting the NX bit, including, but not limited to:
`x86_64`, `x86_32` (with PAE), `ARM` and `ARM64`.

Parts of WX Protection are inspired by some of the features available in PaX.

W^X enforcement
^^^^^^^^^^^^^^^
W^X means that a program can't have a page of memory that is marked, at the
same time, writable and executable. This also allow to detect many bad
behaviours that make life much more easy for attackers. Programs running with
this feature enabled will be more difficult to exploit in the case they are
affected by some vulnerabilities, because the attacker will be forced
to make more steps in order to exploit them.
This feature also blocks accesses to /proc/*/mem files that would allow to
write the current process read-only memory, bypassing any protection.

W!->X (once writable never executable) mprotect restriction
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
"Once writable never executable" means that any page that could have been
marked as writable in the past won't ever be allowed to be marked (e.g. via
an mprotect syscall) as executable.
This goes on the same track as W^X, but is much stricter and prevents
the runtime creation of new executable code in memory.
Obviously, this feature does not prevent a program from creating a new file and
*mmapping* it as executable, however, it will be way more difficult for
attackers to exploit vulnerabilities if this feature is enabled.

Executable MMAP prevention
^^^^^^^^^^^^^^^^^^^^^^^^^^
This feature prevents the creation of new executable mmaps after the dynamic
libraries have been loaded. When used in combination with **W!->X mprotect
restriction** this feature will completely prevent the creation of new
executable code from the current thread.
Obviously, this feature does not prevent cases in which an attacker uses an
*execve* to start a completely new program. This kind of restriction, if
needed, can be applied using one of the other LSM that focuses on MAC.
Please be aware that this feature can break many programs and so it should be
enabled after careful evaluation.

/proc/PID/attr/sara/wxprot interface
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The `procattr` interface can be used by a thread to discover which
WX Protection features are enabled and/or to tighten them: protection
can't be softened via procattr.
The interface is simple: it's a text file with an hexadecimal
number in it representing enabled features (more information can be
found in the `Flags values`_ section). Via this interface it is also
possible to perform a complete memory scan to remove the write permission
from pages that are both writable and executable.

Protections that prevent the runtime creation of executable code
can be troublesome for all those programs that actually need to do it
e.g. programs shipping with a JIT compiler built-in.
This feature can be use to run the JIT compiler with few restrictions
while enforcing full WX Protection in the rest of the program.

The preferred way to access this interface is via `libsara` [2]_.
If you don't want it as a dependency, you can just statically link it
in your project or copy/paste parts of it.
To make things simpler `libsara` is the only part of S.A.R.A. released under
*CC0 - No Rights Reserved* license.

Extended filesystem attributes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
When this functionality is enabled, it's possible to override
WX Protection flags set in the main configuration via extended attributes,
even when S.A.R.A.'s configuration is in "locked" mode.
If the user namespace is also enabled, its attributes will override settings
configured via the security namespace.
The xattrs currently in use are:

- security.sara.wxprot
- user.sara.wxprot

They can be manually set to the desired value as a decimal, hexadecimal or
octal number. When this functionality is enabled, S.A.R.A. can be easily used
without the help of its userspace tools. Though the preferred way to change
these attributes is `sara-xattr` which is part of `saractl` [1]_.

Trampoline emulation
^^^^^^^^^^^^^^^^^^^^
Some programs need to generate part of their code at runtime. Luckily enough,
in some cases they only generate well-known code sequences (the
*trampolines*) that can be easily recognized and emulated by the kernel.
This way WX Protection can still be active, so a potential attacker won't be
able to generate arbitrary sequences of code, but just those that are
explicitly allowed. This is not ideal, but it's still better than having WX
Protection completely disabled.

In particular S.A.R.A. is able to recognize trampolines used by GCC for nested
C functions and libffi's trampolines.
This feature is available only on `x86_32` and `x86_64`.

Flags values
^^^^^^^^^^^^
Flags are represented as a 16 bit unsigned integer in which every bit indicates
the status of a given feature:

+------------------------------+----------+
|           Feature            |  Value   |
+==============================+==========+
| W!->X Heap                   |  0x0001  |
+------------------------------+----------+
| W!->X Stack                  |  0x0002  |
+------------------------------+----------+
| W!->X Other memory           |  0x0004  |
+------------------------------+----------+
| W^X                          |  0x0008  |
+------------------------------+----------+
| Don't enforce, just complain |  0x0010  |
+------------------------------+----------+
| Be Verbose                   |  0x0020  |
+------------------------------+----------+
| Executable MMAP prevention   |  0x0040  |
+------------------------------+----------+
| Force W^X on setprocattr     |  0x0080  |
+------------------------------+----------+
| Trampoline emulation         |  0x0100  |
+------------------------------+----------+
| Children will inherit flags  |  0x0200  |
+------------------------------+----------+

.. [1] `saractl		<https://github.com/smeso/saractl>`_
.. [2] `libsara		<https://github.com/smeso/libsara>`_
.. [3] `sara-test	<https://github.com/smeso/sara-test>`_
.. [4] `Homepage	<https://smeso.it/sara>`_

Bugs
====

Please report any issue to the relevant issue tracker:

* `saractl	<https://github.com/smeso/saractl/issues>`_
* `libsara	<https://github.com/smeso/libsara/issues>`_
* `sara-test	<https://github.com/smeso/sara-test/issues>`_
* `kernel	<https://github.com/smeso/sara/issues>`_
