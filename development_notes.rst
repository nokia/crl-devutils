.. Copyright (C) 2019, Nokia

=======================================
Development notes for crl.devutils
=======================================

Version handling
================

Version handling should take care of the following aspects:

- git tag (which is the package version)

- git hash (should be associated with git tag)

- version (and name) according to setup

::
        # python setup.py --name
        # python setup.py --version


- version according to _version.py file


All of these should be verified to be consistent.
