sara.h manual page
==================

Synopsys
--------
**#include <sara.h>**

Description
-----------
*libsara* can be used to query or change S.A.R.A.'s WX Protections flags.
Please remember that protection can't be softened via this interface and
you will only be allowed to add or remove a flag if and only if that action
won't make the thread less secure.
The VERBOSE flag can't be changed and any attempt to do this will be silently
ignored. Any other violation will result in an error.
Please always check the return value of ``set_wxprot_self_flags``,
``add_wxprot_self_flags`` and ``rm_wxprot_self_flags``.
A thread can only query its own flags, unless it has CAP_MAC_ADMIN.
Also, it can only change its own flags, regardless of the capabilities it has.
Be aware that, while the flags change will only affect the current thread,
the changes forced by ``SARA_FORCE_WXORX`` will affect the whole process.
For more information please see :manpage:`sara(7)`.

The <sara.h> header shall define the following::

	SARA_ERROR
	SARA_HEAP
	SARA_STACK
	SARA_OTHER
	SARA_WXORX
	SARA_COMPLAIN
	SARA_VERBOSE
	SARA_MMAP
	SARA_FORCE_WXORX
	SARA_EMUTRAMP
	SARA_TRANSFER
	SARA_NONE
	SARA_MPROTECT
	SARA_FULL

The following shall be declared as functions and may also be defined
as macros. Function prototypes shall be provided.

.. code-block:: c

	int set_wxprot_self_flags(uint16_t flags);
	int add_wxprot_self_flags(uint16_t flags);
	int rm_wxprot_self_flags(uint16_t flags);
	uint16_t get_wxprot_flags(pid_t pid);
	uint16_t get_wxprot_self_flags(void);
	int is_emutramp_active(void);

See also
--------

:manpage:`sara(7)`, :manpage:`wxprot.conf(5)`, :manpage:`saractl(8)`,
:manpage:`sara-xattr(8)` and <https://sara.smeso.it>
