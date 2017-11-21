Install
*******

As of today S.A.R.A. it's still out of tree, so, if you want to use it,
you'll need to use a patched kernel image or patch the kernel yourself.

-------------------------------------------------------------------------------

Generic Instructions
====================

The stable patch releases can be found `here
<https://github.com/smeso/sara/releases/latest>`_.
The releases are signed with the following PGP key:
``0xD7286260BBF31719A2759FA485F0580B9DACBE6E``.
To apply the patches change the working directory to that of your kernel
sources and enter the usual patch command:

.. code-block:: bash

	$ cd kernel-sources/
	$ patch -p1 < ../sara.patch

where ``../sara.patch`` is the path to the uncompressed and PGP-verified
patch.

To install `saractl` you can use ``pip``:

.. code-block:: bash

	$ pip install saractl

Or you can manually download the latest archive from `here
<https://pypi.python.org/pypi/saractl>`_, check its PGP signature, extract it
and install it by simply running:

.. code-block:: bash

	$ python3 setup.py install

You we'll need at least Python 3.4.
Some saractl's functionalities depend on ``pyelftools``, ``pythonprctl``
and ``pyxattr``. You may want to install them too, but they are optional.
Alternatively you can download the sources tarball from `here
<https://github.com/smeso/saractl/releases/latest>`_ too.

To install the other userspace tools just download the latest GitHub releases
from the links below and follow the instructions in their README file.
Please, always remeber to check PGP signatures.

* `libsara	<https://github.com/smeso/libsara/releases/latest>`_
* `sara-test	<https://github.com/smeso/sara-test/releases/latest>`_
