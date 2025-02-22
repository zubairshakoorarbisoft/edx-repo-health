===============
edx-repo-health
===============

edx-repo-health is a set of checks of the health of repositories.  They are written similar to pytest tests, and run in pytest using a customized pytest plugin (`pytest-repo-health`_).

- Each check gathers information about a given repository.
- Each check is meant to be minimal and only check for simple specific things. This is to make it easier to debug changes.
- Unlike test functions, checks should not use asserts.
- Each check writes its result to a dictionary.
- The completed dictionary is written as a YAML file when all the checks have
  been run.

Installation
------------

1. git clone this repository::

   $ git clone git@github.com:edx/edx-repo-health.git

2. run ``make requirements`` to install reqs for this repo
3. run ``pip install -e .`` to install this project

Usage
-----

- Follow Installation steps above

- cd into the directory you wish to run checks on

- Set credentials:

  - Export ``GITHUB_TOKEN`` into environment, containing GitHub personal API token with read access to edx org.
  - Export ``READTHEDOCS_API_KEY`` into the environment, with a Read The Docs API token.
  - Note: a ~/.netrc file will override these settings and may interfere. You can use ``NETRC=/dev/null`` to hide the file if needed.

- Run the ``run_checks`` command

  ``run_checks`` is a CLI wrapper around pytest and pytest-repo-health_. Running ``run_checks`` will run all checks located in edx-repo-health repo on the directory you are currently cd'd into.  The results will be written into a repo_health.yaml file.

  If you would like to add further flags to the pytest call, you can add the flags after ``run_checks`` command and they will be passed on the pytest automatically. Find out about additional options at pytest-repo-health_ or run ``pytest --help`` command.

Design
------

``edx-repo-health`` is part of a set of tools that together collect results of checks against multiple repositories. Then, taking the individual results as input, it can output the data in different formats including aggregated views of the data.

The following describes the repo health pipeline that currently uses ``edx-repo-health`` (some of the following is private to edx.org and is mentioned because we think it is useful information to have):

- ``edx-repo-health`` contains all the repo health checks that run on edX repositories

  - `repo_health_dashboard script`_  aggregates data generated on each repository by ``org-repo-health-report`` jenkins job into a CSV report

    - this script would also allow you to create other custom dashboards


- `pytest-repo-health`_ repository contains the pytest plugin that allows us to run these checks on a repository

- `org-repo-health-report`_ jenkins job runs all the checks defined in this repo on repositories in edX github organization (private to edx.org).

  - `org-repo-health-report job definition`_ (private to edx.org)

- `repo-health-data`_ repository contains all the data generated by repo health pipeline (private to edx.org).

  - On every merge to master, a GitHub action updates info in `Repo Health Dashboard`_ spreadsheet

- `Repo Health Dashboard`_ is the end result of this pipeline google spreadsheet (private to edx.org).

.. _org-repo-health-report: https://tools-edx-jenkins.edx.org/job/RepoHealth/job/org-repo-health-report/
.. _org-repo-health-report job definition: https://github.com/edx/jenkins-job-dsl-internal/blob/master/jobs/tools-edx-jenkins.edx.org/createRepoHealthJobs.groovy
.. _repo_health_dashboard script: https://github.com/openedx/edx-repo-health/blob/master/repo_health_dashboard/repo_health_dashboard.py
.. _repo-health-data: https://github.com/openedx/repo-health-data
.. _Repo Health Dashboard: https://docs.google.com/spreadsheets/d/1VCxNVq-niT-uv5BFmsYPF21r6I2-IQ-GJbidF0zUPBc/edit#gid=921158295


Adding Checks
-------------

For details on adding new checks, please See `how_tos`_.

Future improvements
-------------------

- Documenting standard reqs/checks in each check better.

- Create tests for the checks and make sure they behave correctly for different dir/file structure.


Contributing
------------

Contributions are very welcome. Tests can be run with `tox`_, please ensure
the coverage at least stays the same before you submit a pull request.


License
-------

The code in this repository is licensed under the Apache Software License 2.0 unless
otherwise noted.

Please see ``LICENSE.txt`` for details.


Reporting Security Issues
-------------------------

Please do not report security issues in public. Please email security@edx.org.


Getting Help
------------

Have a question about this repository, or about Open edX in general?  Please
refer to this `list of resources`_ if you need any assistance.

.. _list of resources: https://open.edx.org/getting-help
.. _pytest-repo-health: https://github.com/openedx/pytest-repo-health
.. _how_tos: https://github.com/openedx/edx-repo-health/blob/master/docs/how_tos/add_checks.rst
.. _`file an issue`: https://github.com/openedx/edx-repo-health/issues
.. _`pytest`: https://github.com/pytest-dev/pytest
.. _`tox`: https://tox.readthedocs.io/en/latest/
