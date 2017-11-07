Kernel Configuration
********************

**CONFIG_SECURITY_SARA** - Enable S.A.R.A.

	This selects S.A.R.A. LSM, which aims to collect heterogeneous
	security measures providing a common interface to manage them.
	This LSM will always be stacked with the selected primary LSM and
	other stacked LSMs.


**CONFIG_SECURITY_SARA_DEFAULT_DISABLED** -
S.A.R.A. will be disabled at boot

	If you say Y here, S.A.R.A. will not be enabled at startup.
	You can override this option at boot time via "sara.enabled=[1|0]"
	kernel parameter or via user-space utilities.
	This option is useful for distro kernels.


**CONFIG_SECURITY_SARA_NO_RUNTIME_ENABLE** -
S.A.R.A. can be turn on only at boot time

	By enabling this option it won't be possible to turn on S.A.R.A.
	at runtime via user-space utilities. However it can still be
	turned on at boot time via the "sara.enabled=1" kernel parameter.
	This option is functionally equivalent to "sara.enabled=0" kernel
	parameter. This option is useful for distro kernels.

**CONFIG_SECURITY_SARA_WXPROT** -
WX Protection: W^X and W!->X protections

	WX Protection aims to improve user-space programs security by applying:

	* W^X memory restriction
	* W!->X (once writable never executable) mprotect restriction
	* Executable MMAP prevention
	
	See `WX Protection`_.

**Default action for W^X and W!->X protections**

	Choose the default behaviour of WX Protection when no config
	rule matches or no rule is loaded.

	**CONFIG_SECURITY_SARA_WXPROT_DEFAULT_FLAGS_ALL_COMPLAIN_VERBOSE** -
	Protections enabled but not enforced

		All features enabled except "Executable MMAP prevention",
		verbose reporting, but no actual enforce: it just complains.
		Its numeric value is 0x3f. See `Flags values`_.

        **CONFIG_SECURITY_SARA_WXPROT_DEFAULT_FLAGS_ALL_ENFORCE_VERBOSE** -
	Full protection, verbose

		All features enabled except "Executable MMAP prevention".
		The enabled features will be enforced with verbose reporting.
		Its numeric value is 0x2f. See `Flags values`_.

        **CONFIG_SECURITY_SARA_WXPROT_DEFAULT_FLAGS_ALL_ENFORCE** -
	Full protection, quiet

		All features enabled except "Executable MMAP prevention".
		The enabled features will be enforced quietly.
		Its numeric value is 0xf. See `Flags values`_.

	**CONFIG_SECURITY_SARA_WXPROT_DEFAULT_FLAGS_NONE** -
	No protection at all

		All features disabled.
		Its numeric value is 0. See `Flags values`_.

**CONFIG_SECURITY_SARA_WXPROT_EMUTRAMP** -
Enable emulation for some types of trampolines

	Some programs and libraries need to execute special small code
	snippets from non-executable memory pages.
	Most notable examples are the GCC and libffi trampolines.
	This features make it possible to execute those trampolines even
	if they reside in non-executable memory pages.
	This features need to be enabled on a per-executable basis
	via user-space utilities.  See `Trampoline emulation`_.


**CONFIG_SECURITY_SARA_WXPROT_XATTRS_ENABLED** -
xattrs support enabled by default

	If you say Y here it will be possible to override WX protection
	configuration via extended attributes in the security namespace.
	Even when S.A.R.A.'s configuration has been locked. See
	`Extended filesystem attributes`_.


**CONFIG_CONFIG_SECURITY_SARA_WXPROT_XATTRS_USER** -
'user' namespace xattrs support enabled by default

	If you say Y here it will be possible to override WX protection
	configuration via extended attributes in the user namespace.
	Even when S.A.R.A.'s configuration has been locked. See
	`Extended filesystem attributes`_.


**CONFIG_SECURITY_SARA_WXPROT_DISABLED** -
WX protection will be disabled at boot

	If you say Y here WX protection won't be enabled at startup. You can
	override this option via user-space utilities or at boot time via
	"sara.wxprot_enabled=[0|1]" kernel parameter.


Kernel Parameters
*****************

**sara.enabled=** Disable or enable S.A.R.A. at boot time.

		If disabled this way, S.A.R.A. can't be enabled
		again.

		Format: { "0" | "1" }

		See `Kernel Configuration`_

		0 -- disable.

		1 -- enable.

		Default value is set via kernel config option.


**sara.wxprot_enabled=** Disable or enable S.A.R.A. WX Protection
at boot time.

		Format: { "0" | "1" }

		See `Kernel Configuration`_

		0 -- disable.

		1 -- enable.

		Default value is set via kernel config option.

**sara.wxprot_default_flags=** Set S.A.R.A. WX Protection default flags.

		Format: <integer>

		See `Flags values`_

		Default value is set via kernel config option.

**sara.wxprot_xattrs_enabled=** Enable support for security xattrs.

		Format: { "0" | "1" }

		See `Kernel Configuration`_

		0 -- disable.

		1 -- enable.

		Default value is set via kernel config option.

**sara.wxprot_xattrs_user=** Enable support for user xattrs.

		Format: { "0" | "1" }

		See `Kernel Configuration`_

		0 -- disable.

		1 -- enable.

		Default value is set via kernel config option.
