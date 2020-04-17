Description
--------------------------------------------------------------

Ansible role to apply system wide configuration.

This role can take multiple git repository URLs and clone each repository
to the */* (root) directory of the system.

This role performs the following actions:

- Ensure the requirements are installed.

- Ensure the current user can obtain administrative (root) permissions.

- Update the apt cache.

- Ensure dependencies are installed.

- If the **system_skeleton** variable is defined, clone the git repositories
  listed on it into */*.

- If the **configuration** variable is defined, clone the git system
  repositories listed on it into */*.

This role expand files or URLs by default so you must write your items like:

.. substitution-code-block:: bash

 |DEFAULT_VAR_NAME|:
   - item_path: |DEFAULT_VAR_VALUE|
     item_expand: false


You can change this default behaviour by:

- Setting the **expand** variable to *false*.

Or

- Add to an item the attribute **item_expand** setted to *true*.