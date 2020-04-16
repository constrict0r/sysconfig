
sysconfig
*********

.. image:: https://gitlab.com/constrict0r/sysconfig/badges/master/pipeline.svg
   :alt: pipeline

.. image:: https://travis-ci.com/constrict0r/sysconfig.svg
   :alt: travis

.. image:: https://readthedocs.org/projects/sysconfig/badge
   :alt: readthedocs

Ansible to apply system-wide configuration.

.. image:: https://gitlab.com/constrict0r/img/raw/master/sysconfig/avatar.png
   :alt: avatar

Full documentation on `Readthedocs
<https://sysconfig.readthedocs.io>`_.

Source code on:

`Github <https://github.com/constrict0r/sysconfig>`_.

`Gitlab <https://gitlab.com/constrict0r/sysconfig>`_.

`Part of: <https://gitlab.com/explore/projects?tag=doombot>`_

.. image:: https://gitlab.com/constrict0r/img/raw/master/sysconfig/doombot.png
   :alt: doombot

**Ingredients**

.. image:: https://gitlab.com/constrict0r/img/raw/master/sysconfig/ingredient.png
   :alt: ingredient


Contents
********

* `Description <#Description>`_
* `Usage <#Usage>`_
* `Variables <#Variables>`_
   * `expand <#expand>`_
   * `system_skeleton <#system-skeleton>`_
   * `configuration <#configuration>`_
* `YAML <#YAML>`_
* `Attributes <#Attributes>`_
   * `item_expand <#item-expand>`_
   * `item_path <#item-path>`_
* `Requirements <#Requirements>`_
* `Compatibility <#Compatibility>`_
* `License <#License>`_
* `Links <#Links>`_
* `UML <#UML>`_
   * `Deployment <#deployment>`_
   * `Main <#main>`_
* `Author <#Author>`_

Description
***********

Ansible role to apply system wide configuration.

This role can take multiple git repository URLs and clone each
repository to the */* (root) directory of the system.

This role performs the following actions:

* Ensure the requirements are installed.

* Ensure the current user can obtain administrative (root)
   permissions.

* Update the apt cache.

* Ensure dependencies are installed.

* If the **system_skeleton** variable is defined, clone the git
   repositories listed on it into */*.

* If the **configuration** variable is defined, clone the git system
   repositories listed on it into */*.

This role do not expand files or URLs by default because the most
common case is to specify URLs that points directly to a skeleton
repository, so the default behaviour for this role is to treat file
paths and URLs as plain text.

You can change the default behaviour by:

* Setting the **expand** variable to *true*.

Or

* Add to an item the attribute **item_expand** setted to *true*.



Usage
*****

* To install and execute:

..

   ::

      ansible-galaxy install constrict0r.sysconfig
      ansible localhost -m include_role -a name=constrict0r.sysconfig -K

* Passing variables:

..

   ::

      ansible localhost -m include_role -a name=constrict0r.sysconfig -K \
          -e "{system_skeleton: ['https://gitlab.com/huertico/server']}"

* To include the role on a playbook:

..

   ::

      - hosts: servers
        roles:
            - {role: constrict0r.sysconfig}

* To include the role as dependency on another role:

..

   ::

      dependencies:
        - role: constrict0r.sysconfig
          system_skeleton: ['https://gitlab.com/huertico/server']

* To use the role from tasks:

..

   ::

      - name: Execute role task.
        import_role:
          name: constrict0r.sysconfig
        vars:
          system_skeleton: ['https://gitlab.com/huertico/server']

To run tests:

::

   cd sysconfig
   chmod +x testme.sh
   ./testme.sh

On some tests you may need to use *sudo* to succeed.



Variables
*********

The following variables are supported:


expand
======

Boolean value indicating if load items from file paths or URLs or just
treat files and URLs as plain text.

If set to *true* this role will attempt to load items from the
especified paths and URLs.

If set to *false* each file path or URL found on system_skeleton will
be treated as plain text.

This variable is set to *false* by default.

::

   ansible localhost -m include_role -a name=constrict0r.sysconfig \
       -e "expand=true configuration='/home/username/my-config.yml' titles='system_skeleton'"

If you wish to override the value of this variable, specify an
*item_path* and an *item_expand* attributes when passing the item, the
*item_path* attribute can be used with URLs too:

::

   ansible localhost -m include_role -a name=constrict0r.sysconfig \
       -e "{expand: false,
           system_skeleton: [ \
               item_path: '/home/username/my-config.yml', \
               item_expand: false \
           ], titles: 'system_skeleton'}"

To prevent any unexpected behaviour, it is recommended to always
specify this variable when calling this role.


system_skeleton
===============

URL or list of URLs pointing to git skeleton repositories containing
layouts of directories and configuration files.

Each URL on system_skeleton will be checked to see if it points to a
valid git repository, and if it does, the git repository is cloned.

The contents of each cloned repository will then be copied to the root
of the filesystem as a simple method to apply system-wide
configuration.

This variable is empty by default.

::

   # Including from terminal.
   ansible localhost -m include_role -a name=constrict0r.sysconfig -K -e \
       "{system_skeleton: [https://gitlab.com/huertico/server]}"

   # Including on a playbook.
   - hosts: servers
     roles:
       - role: constrict0r.sysconfig
         system_skeleton:
           - https://gitlab.com/huertico/server
           - https://gitlab.com/huertico/client

   # To a playbook from terminal.
   ansible-playbook -i tests/inventory tests/test-playbook.yml -K -e \
       "{system_skeleton: [https://gitlab.com/huertico/server]}"


configuration
=============

Absolute file path or URL to a *.yml* file that contains all or some
of the variables supported by this role.

It is recommended to use a *.yml* or *.yaml* extension for the
**configuration** file.

This variable is empty by default.

::

   # Using file path.
   ansible localhost -m include_role -a name=constrict0r.sysconfig -K -e \
       "configuration=/home/username/my-config.yml"

   # Using URL.
   ansible localhost -m include_role -a name=constrict0r.sysconfig -K -e \
       "configuration=https://my-url/my-config.yml"

To see how to write  a configuration file see the *YAML* file format
section.



YAML
****

When passing configuration files to this role as parameters, it’s
recommended to add a *.yml* or *.yaml* extension to the each file.

It is also recommended to add three dashes at the top of each file:

::

   ---

You can include in the file the variables required for your tasks:

::

   ---
   system_skeleton:
     - ['https://gitlab.com/huertico/server']

If you want this role to load list of items from files and URLs you
can set the **expand** variable to *true*:

::

   ---
   system_skeleton: /home/username/my-config.yml

   expand: true

If the expand variable is *false*, any file path or URL found will be
treated like plain text.



Attributes
**********

On the item level you can use attributes to configure how this role
handles the items data.

The attributes supported by this role are:


item_expand
===========

Boolean value indicating if treat this item as a file path or URL or
just treat it as plain text.

::

   ---
   system_skeleton:
     - item_expand: true
       item_path: /home/username/my-config.yml


item_path
=========

Absolute file path or URL to a *.yml* file.

::

   ---
   system_skeleton:
     - item_path: /home/username/my-config.yml

This attribute also works with URLs.



Requirements
************

* `Ansible <https://www.ansible.com>`_ >= 2.8.

* `Jinja2 <https://palletsprojects.com/p/jinja/>`_.

* `Pip <https://pypi.org/project/pip/>`_.

* `Python <https://www.python.org/>`_.

* `PyYAML <https://pyyaml.org/>`_.

* `Requests <https://2.python-requests.org/en/master/>`_.

If you want to run the tests, you will also need:

* `Docker <https://www.docker.com/>`_.

* `Molecule <https://molecule.readthedocs.io/>`_.

* `Setuptools <https://pypi.org/project/setuptools/>`_.



Compatibility
*************

* `Debian Buster <https://wiki.debian.org/DebianBuster>`_.

* `Debian Raspbian <https://raspbian.org/>`_.

* `Debian Stretch <https://wiki.debian.org/DebianStretch>`_.

* `Ubuntu Xenial <http://releases.ubuntu.com/16.04/>`_.



License
*******

MIT. See the LICENSE file for more details.



Links
*****

* `Github <https://github.com/constrict0r/sysconfig>`_.

* `Gitlab <https://gitlab.com/constrict0r/sysconfig>`_.

* `Gitlab CI <https://gitlab.com/constrict0r/sysconfig/pipelines>`_.

* `Readthedocs <https://sysconfig.readthedocs.io>`_.

* `Travis CI <https://travis-ci.com/constrict0r/sysconfig>`_.



UML
***


Deployment
==========

The full project structure is shown below:

.. image:: https://gitlab.com/constrict0r/img/raw/master/sysconfig/deploy.png
   :alt: deploy


Main
====

The project data flow is shown below:

.. image:: https://gitlab.com/constrict0r/img/raw/master/sysconfig/main.png
   :alt: main



Author
******

.. image:: https://gitlab.com/constrict0r/img/raw/master/sysconfig/author.png
   :alt: author

The Travelling Vaudeville Villain.

Enjoy!!!

.. image:: https://gitlab.com/constrict0r/img/raw/master/sysconfig/enjoy.png
   :alt: enjoy


