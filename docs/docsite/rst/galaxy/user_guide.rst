.. _using_galaxy:
.. _ansible_galaxy:

*****************
Galaxy User Guide
*****************

:dfn:`Ansible Galaxy` refers to the `Galaxy <https://galaxy.ansible.com>`_  website, a free site for finding, downloading, and sharing community developed roles.

Use Galaxy to jump-start your automation project with great content from the Ansible community. Galaxy provides pre-packaged units of work such as `roles <playbooks_reuse_roles>`_, and new in Galaxy 3.2, `collections <collections>`_.
You can find roles for provisioning infrastructure, deploying applications, and all of the tasks you do everyday. The collection format provides a comprehensive package of automation that may include multiple playbooks, roles, modules, and plugins.

.. contents::
   :local:
   :depth: 2
.. _finding_galaxy_collections:

Finding collections on Galaxy
=============================

To find collections on Galaxy:

#. Click the :guilabel:`Search` icon in the left-hand navigation.
#. Set the filter to *collection*.
#. Set other filters and press :guilabel:`enter`.

Galaxy presents a list of collections that match your search criteria.

.. _installing_galaxy_collections:


Installing collections
======================


Installing a collection from Galaxy
-----------------------------------

.. include:: ../shared_snippets/installing_collections.txt

.. _installing_ah_collection:

Downloading a collection from Automation Hub
----------------------------------------------------

You can download collections from Automation Hub at the command line. Automation Hub content is available to subscribers only, so you must download an API token and configure your local environment to provide it before you can you download collections. To download a collection from Automation Hub with the ``ansible-galaxy`` command:

1. Get your Automation Hub API token. Go to https://cloud.redhat.com/ansible/automation-hub/token/ and click :guilabel:`Get API token` from the version dropdown to copy your API token.
2. Configure Red Hat Automation Hub server in the ``server_list``  option under the ``[galaxy]`` section in your :file:`ansible.cfg` file.

  .. code-block:: ini

      [galaxy]
      server_list = automation_hub

      [galaxy_server.automation_hub]
      url=https://cloud.redhat.com/api/automation-hub/
      auth_url=https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
      token=my_ah_token

3. Download the collection hosted in Automation Hub.

  .. code-block:: bash

     ansible-galaxy collection install my_namespace.my_collection

.. seealso::
  `Getting started with Automation Hub <https://www.ansible.com/blog/getting-started-with-ansible-hub>`_
    An introduction to Automation Hub

Installing an older version of a collection
-------------------------------------------

.. include:: ../shared_snippets/installing_older_collection.txt

Install multiple collections with a requirements file
-----------------------------------------------------

.. include:: ../shared_snippets/installing_multiple_collections.txt


Configuring the ``ansible-galaxy`` client
------------------------------------------

.. include:: ../shared_snippets/galaxy_server_list.txt

.. _finding_galaxy_roles:

Finding roles on Galaxy
=======================

Search the Galaxy database by tags, platforms, author and multiple keywords. For example:

.. code-block:: bash

    $ ansible-galaxy search elasticsearch --author geerlingguy

The search command will return a list of the first 1000 results matching your search:

.. code-block:: text

    Found 2 roles matching your search:

    Name                              Description
    ----                              -----------
    geerlingguy.elasticsearch         Elasticsearch for Linux.
    geerlingguy.elasticsearch-curator Elasticsearch curator for Linux.


Get more information about a role
---------------------------------

Use the ``info`` command to view more detail about a specific role:

.. code-block:: bash

    $ ansible-galaxy info username.role_name

This returns everything found in Galaxy for the role:

.. code-block:: text

    Role: username.role_name
        description: Installs and configures a thing, a distributed, highly available NoSQL thing.
        active: True
        commit: c01947b7bc89ebc0b8a2e298b87ab416aed9dd57
        commit_message: Adding travis
        commit_url: https://github.com/username/repo_name/commit/c01947b7bc89ebc0b8a2e298b87ab
        company: My Company, Inc.
        created: 2015-12-08T14:17:52.773Z
        download_count: 1
        forks_count: 0
        github_branch:
        github_repo: repo_name
        github_user: username
        id: 6381
        is_valid: True
        issue_tracker_url:
        license: Apache
        min_ansible_version: 1.4
        modified: 2015-12-08T18:43:49.085Z
        namespace: username
        open_issues_count: 0
        path: /Users/username/projects/roles
        scm: None
        src: username.repo_name
        stargazers_count: 0
        travis_status_url: https://travis-ci.org/username/repo_name.svg?branch=master
        version:
        watchers_count: 1


.. _installing_galaxy_roles:

Installing roles from Galaxy
============================

The ``ansible-galaxy`` command comes bundled with Ansible, and you can use it to install roles from Galaxy or directly from a git based SCM. You can
also use it to create a new role, remove roles, or perform tasks on the Galaxy website.

The command line tool by default communicates with the Galaxy website API using the server address *https://galaxy.ansible.com*. Since the `Galaxy project <https://github.com/ansible/galaxy>`_
is an open source project, you may be running your own internal Galaxy server and wish to override the default server address. You can do this using the *--server* option
or by setting the Galaxy server value in your *ansible.cfg* file. For information on setting the value in *ansible.cfg* see :ref:`galaxy_server`.


Installing roles
----------------

Use the ``ansible-galaxy`` command to download roles from the `Galaxy website <https://galaxy.ansible.com>`_

.. code-block:: bash

  $ ansible-galaxy install namespace.role_name

Setting where to install roles
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By default, Ansible downloads roles to the first writable directory in the default list of paths ``~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles``. This installs roles in the home directory of the user running ``ansible-galaxy``.

You can override this with one of the following options:

* Set the environment variable :envvar:`ANSIBLE_ROLES_PATH` in your session.
* Define ``roles_path`` in an ``ansible.cfg`` file.
* Use the ``--roles-path`` option for the ``ansible-galaxy`` command.

The following provides an example of using ``--roles-path`` to install the role into the current working directory:

.. code-block:: bash

    $ ansible-galaxy install --roles-path . geerlingguy.apache

.. seealso::

   :ref:`intro_configuration`
      All about configuration files

Installing a specific version of a role
---------------------------------------

When the Galaxy server imports a role, it imports any git tags matching the Semantic Version format as versions. In turn, you can download a specific version of a role by specifying one of the imported tags.

To see the available versions for a role:

#. Locate the role on the Galaxy search page.
#. Click on the name to view more details, including the available versions.

You can also navigate directly to the role using the /<namespace>/<role name>. For example, to view the role geerlingguy.apache, go to `<https://galaxy.ansible.com/geerlingguy/apache>`_.

To install a specific version of a role from Galaxy, append a comma and the value of a GitHub release tag. For example:

.. code-block:: bash

   $ ansible-galaxy install geerlingguy.apache,v1.0.0

It is also possible to point directly to the git repository and specify a branch name or commit hash as the version. For example, the following will
install a specific commit:

.. code-block:: bash

   $ ansible-galaxy install git+https://github.com/geerlingguy/ansible-role-apache.git,0b7cd353c0250e87a26e0499e59e7fd265cc2f25

Installing multiple roles from a file
-------------------------------------

You can install multiple roles by including the roles in a :file:`requirements.yml` file. The format of the file is YAML, and the
file extension must be either *.yml* or *.yaml*.

Use the following command to install roles included in :file:`requirements.yml:`

.. code-block:: bash

    $ ansible-galaxy install -r requirements.yml

Again, the extension is important. If the *.yml* extension is left off, the ``ansible-galaxy`` CLI assumes the file is in an older, now deprecated,
"basic" format.

Each role in the file will have one or more of the following attributes:

   src
     The source of the role. Use the format *namespace.role_name*, if downloading from Galaxy; otherwise, provide a URL pointing
     to a repository within a git based SCM. See the examples below. This is a required attribute.
   scm
     Specify the SCM. As of this writing only *git* or *hg* are allowed. See the examples below. Defaults to *git*.
   version:
     The version of the role to download. Provide a release tag value, commit hash, or branch name. Defaults to the branch set as a default in the repository, otherwise defaults to the *master*.
   name:
     Download the role to a specific name. Defaults to the Galaxy name when downloading from Galaxy, otherwise it defaults
     to the name of the repository.

Use the following example as a guide for specifying roles in *requirements.yml*:

.. code-block:: yaml

    # from galaxy
    - src: yatesr.timezone

    # from GitHub
    - src: https://github.com/bennojoy/nginx

    # from GitHub, overriding the name and specifying a specific tag
    - src: https://github.com/bennojoy/nginx
      version: master
      name: nginx_role

    # from a webserver, where the role is packaged in a tar.gz
    - src: https://some.webserver.example.com/files/master.tar.gz
      name: http-role-gz

    # from a webserver, where the role is packaged in a tar.bz2
    - src: https://some.webserver.example.com/files/master.tar.bz2
      name: http-role-bz2

    # from a webserver, where the role is packaged in a tar.xz (Python 3.x only)
    - src: https://some.webserver.example.com/files/master.tar.xz
      name: http-role-xz

    # from Bitbucket
    - src: git+https://bitbucket.org/willthames/git-ansible-galaxy
      version: v1.4

    # from Bitbucket, alternative syntax and caveats
    - src: https://bitbucket.org/willthames/hg-ansible-galaxy
      scm: hg

    # from GitLab or other git-based scm, using git+ssh
    - src: git@gitlab.company.com:mygroup/ansible-base.git
      scm: git
      version: "0.1"  # quoted, so YAML doesn't parse this as a floating-point value

Installing multiple roles from multiple files
---------------------------------------------

For large projects, the ``include`` directive in a :file:`requirements.yml` file provides the ability to split a large file into multiple smaller files.

For example, a project may have a :file:`requirements.yml` file, and a :file:`webserver.yml` file.

Below are the contents of the :file:`webserver.yml` file:

.. code-block:: bash

    # from github
    - src: https://github.com/bennojoy/nginx

    # from Bitbucket
    - src: git+http://bitbucket.org/willthames/git-ansible-galaxy
      version: v1.4

The following shows the contents of the :file:`requirements.yml` file that now includes the :file:`webserver.yml` file:

.. code-block:: bash

  # from galaxy
  - src: yatesr.timezone
  - include: <path_to_requirements>/webserver.yml

To install all the roles from both files, pass the root file, in this case :file:`requirements.yml` on the
command line, as follows:

.. code-block:: bash

    $ ansible-galaxy install -r requirements.yml

.. _galaxy_dependencies:

Dependencies
------------

Roles can also be dependent on other roles, and when you install a role that has dependencies, those dependencies will automatically be installed.

You specify role dependencies in the ``meta/main.yml`` file by providing a list of roles. If the source of a role is Galaxy, you can simply specify the role in
the format ``namespace.role_name``. You can also use the more complex format in ``requirements.yml``, allowing you to provide ``src``, ``scm``, ``version``, and ``name``.

The following shows an example ``meta/main.yml`` file with dependent roles:

.. code-block:: yaml

    ---
    dependencies:
      - geerlingguy.java

    galaxy_info:
      author: geerlingguy
      description: Elasticsearch for Linux.
      company: "Midwestern Mac, LLC"
      license: "license (BSD, MIT)"
      min_ansible_version: 2.4
      platforms:
      - name: EL
        versions:
        - all
      - name: Debian
        versions:
        - all
      - name: Ubuntu
        versions:
        - all
      galaxy_tags:
        - web
        - system
        - monitoring
        - logging
        - lucene
        - elk
        - elasticsearch

Tags are inherited *down* the dependency chain. In order for tags to be applied to a role and all its dependencies, the tag should be applied to the role, not to all the tasks within a role.

Roles listed as dependencies are subject to conditionals and tag filtering, and may not execute fully depending on
what tags and conditionals are applied.

If the source of a role is Galaxy, specify the role in the format *namespace.role_name*:

.. code-block:: yaml

    dependencies:
      - geerlingguy.apache
      - geerlingguy.ansible


Alternately, you can specify the role dependencies in the complex form used in  :file:`requirements.yml` as follows:

.. code-block:: yaml

    dependencies:
      - src: geerlingguy.ansible
      - src: git+https://github.com/geerlingguy/ansible-role-composer.git
        version: 775396299f2da1f519f0d8885022ca2d6ee80ee8
        name: composer

When dependencies are encountered by ``ansible-galaxy``, it will automatically install each dependency to the ``roles_path``. To understand how dependencies are handled during play execution, see :ref:`playbooks_reuse_roles`.

.. note::

    Galaxy expects all role dependencies to exist in Galaxy, and therefore dependencies to be specified in the
    ``namespace.role_name`` format. If you import a role with a dependency where the ``src`` value is a URL, the import process will fail.

List installed roles
--------------------

Use ``list`` to show the name and version of each role installed in the *roles_path*.

.. code-block:: bash

    $ ansible-galaxy list
      - ansible-network.network-engine, v2.7.2
      - ansible-network.config_manager, v2.6.2
      - ansible-network.cisco_nxos, v2.7.1
      - ansible-network.vyos, v2.7.3
    - ansible-network.cisco_ios, v2.7.0

Remove an installed role
------------------------

Use ``remove`` to delete a role from *roles_path*:

.. code-block:: bash

    $ ansible-galaxy remove namespace.role_name


.. seealso::
  `collections <collections>`_
    Sharable collections of modules, playbooks and roles
  `roles <playbooks_reuse_roles>`_
    Reusable tasks, handlers, and other files in a known directory structure
