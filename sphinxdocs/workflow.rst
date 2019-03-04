Workflow
========

Workflow of the Common Robot Libraries development is illustraded by the
following figure:

.. graphviz::

   digraph {
      rankdir=TB
      label="Workflow";
      node [style=filled]
      subgraph sequence {
         node [shape=circle, color=lightblue];
         start [label="Start"];
         stop  [label="Stop"];
         node [shape=box, color=lightblue];
         create [label="1. Create issue"];
         fork [label="2. Fork repository"];
         pull [label="3. Create pull request"];
         node [color=lightcoral]
         travis [label="4. Travis CI"];
         testing [label="5. Library testing"];
         node [shape=box, color=wheat];
         review [label="6. Review"];
         merge [label="7. Reabse and merge"];
         newversion [label="8. Is version\nupdated?", color=lightcoral, shape=diamond];
         node [shape=box, color=lightcoral];
         tag [label="9. Tag release version"];
         upload [label="10. Upload to PyPI"];
         pypi [shape=cylinder, color=whitesmoke, label="PyPI"];
         start -> create -> fork -> pull -> {travis testing review}
         review -> merge -> newversion;
         newversion -> tag [label="yes"];
         newversion -> stop [label="no"];
         tag -> upload -> {pypi, stop};
         {rank=same; create; fork; pull};
         {rank=same; travis; testing; review};
         {rank=same; upload; pypi}
      }
      subgraph roles {
         node [shape=box]
         contributor [label="Contributor", color=lightblue]
         ci [label="Automation", color=lightcoral]
         reviewer [label="Reviewer", color=wheat]
         {rank=same; contributor;ci;reviewer}
      }
   }

1. Create issue
---------------

Please create issue via issues page of library. For example, for
crl-examplelib, please use https://github.com/nokia/crl-examplelib/issues.

2. Fork repository
------------------

Please fork repository to your personal account (no branching supported). If
already forked, you can continue using the old fork.

3. Create pull request
----------------------

Create pull request of the repository.

4. Travis CI
------------

Each library should contain .travis.yml configuration for *tox* tests. This is executed
by `Travis CI`_ automatically.

.. _`Travis CI`: https://travis-ci.org/

5. Library testing
------------------

External automation CI can be used for running *crl test* for the library.
Library should contain *tox* configuration for running this test. This test
creates a test index to Devpi_ server, uploads the documentation and tests the
uploaded distribution with *tox*. This index can be used in further testing the
library when applying the change into test automation frameworks.

.. _Devpi: https://devpi.net/docs/devpi/devpi/stable/%2Bd/index.html

6. Review
---------

GitHub code review process is used for discussion and correction of the change.

7. Rebase and Merge
-------------------

Reviewer or the author of the repository should rebase and merge the pull
request when all tests are passing and reviewers have voted the change to be
approved.

8. Is version updated?
----------------------

Each library contains manually edited CHANGES file (either CHANGES.md or
CHANGES.rst) and _version.py file. The top most version of the CHANGES must
match with the _version.py file version. The version update may contain update
to the version but that is not required. The change should be described in both
commit message and in the CHANGES file.

The version scheme follows PEP-440_ and the following:

  - A.B.C, where A is major version, B is minor version, C is for bug fixes

  - A.B.CbD, where D is beta version number of version A.B.C

The following conventions are used for version updates:

- Major version changes may change interfaces and is not necessarily backward
  compatible.

- Minor version change is for backward compatible updates in the interfaces

- Bug fixes should not update interfaces but merely internal business logic.

.. _PEP-440: https://www.python.org/dev/peps/pep-0440

9. Tag release version
----------------------

If version is updated, then tag for the version number is created and pushed.
This can be done using command *crl tag_setup_version*.

10. Upload to PyPI
------------------

All releases are uploaded to PyPI_.

.. _PyPI: https://pypi.org
