sara-test manual page
=====================

Synopsys
--------
**sara-test**

Description
-----------
:program:`sara-test` is a regression test tool for S.A.R.A.
When S.A.R.A. is disabled the output should look like this::

	These tests should pass even with SARA disabled:
	              wx_mappings:      OK
	             nx_shellcode:      OK
	     fake_trampoline_heap:      OK
	 gcc_trampolines_working1:      OK
	 gcc_trampolines_working2:      OK

	These tests should pass with SARA fully enabled:
	             anon_mmap_wx:      VULNERABLE
	             file_mmap_wx:      VULNERABLE
	     gnu_executable_stack:      VULNERABLE
	            heap_mprotect:      VULNERABLE
	           stack_mprotect:      VULNERABLE
	       anon_mmap_mprotect:      VULNERABLE
	       file_mmap_mprotect:      VULNERABLE
	            text_mprotect:      VULNERABLE
	             bss_mprotect:      VULNERABLE
	            data_mprotect:      VULNERABLE
	                mmap_exec:      VULNERABLE
	                 transfer:      ERROR
	         fake_trampolines:      ERROR

	Tests for procattr interface:
	         correct_settings:      VULNERABLE
	procattr tests disabled

When S.A.R.A. is enabled the output should look like this::


	These tests should pass even with SARA disabled:
	              wx_mappings:      OK
	             nx_shellcode:      OK
	     fake_trampoline_heap:      OK
	 gcc_trampolines_working1:      OK
	 gcc_trampolines_working2:      OK
	
	These tests should pass with SARA fully enabled:
	             anon_mmap_wx:      OK
	             file_mmap_wx:      OK
	     gnu_executable_stack:      OK
	            heap_mprotect:      OK
	           stack_mprotect:      OK
	       anon_mmap_mprotect:      OK
	       file_mmap_mprotect:      OK
	            text_mprotect:      OK
	             bss_mprotect:      OK
	            data_mprotect:      OK
	                mmap_exec:      OK
	                 transfer:      OK
	         fake_trampolines:      OK
	
	Tests for procattr interface:
	         correct_settings:      OK
	         verbosity_change:      OK
	          complain_change:      OK
	     full_change_no_force:      OK
	              force_wxorx:      OK

Please note that, in case you are running on an architecture that lacks
trampoline emulation, the results of the following tests will be ``NOT AVAILABLE``:

* fake_trampoline_heap
* gcc_trampolines_working1
* gcc_trampolines_working2
* fake_trampolines

If you get any result different from what is listed above please open
an issue at <https://github.com/smeso/sara-test/issues>.

Bugs
----
If you find any bugs, please report them at
<https://github.com/smeso/sara-test/issues>

See also
--------

:manpage:`sara(7)`, :manpage:`wxprot.conf(5)`, :manpage:`saractl(8)`
and <https://sara.smeso.it>
