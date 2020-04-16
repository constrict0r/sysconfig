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

This role do not expand files or URLs by default because the most common case
is to specify URLs that points directly to a skeleton repository, so the
default behaviour for this role is to treat file paths and URLs as plain text.

You can change the default behaviour by:

- Setting the **expand** variable to *true*.

Or

- Add to an item the attribute **item_expand** setted to *true*.