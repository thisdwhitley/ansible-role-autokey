# Ansible Role: autokey

This is an Ansible role to install the
[autokey](https://github.com/autokey/autokey) desktop automation utility for
Linux and X11.

I prefer to pass the variables "into" the role from the playbook versus by
including variable files.  This is because I hope to make the role usable by
other projects/roles.  I don't know if this logic makes sense or not, but I am
essentially attempting to remove the variables from the role itself.

This utility does not currently have a flatpak.

## Overview

At a very high level, this role will:

* install the package from a repository
* [*optional*] if a user and git repo link is provided, attempt to copy down the
  repository for the user

I am sure that it is possible to use this utility on other OSes, but I lost my
patience in trying to get it to work, so I have only tested it on Fedora:

* fedora27
* fedora28
* fedora29

The testing of this role is very specific to the role I've set up in molecule,
but I think I'm ok with that.

## Important Notes

* If a git repository is specified, it should be able to be accessed by the role
  (either by a public repo or already having SSH keys populated)

## Requirements

Any package or additional repository requirements will be addressed in the role.

## Role Variables

All of these variables should be considered **optional** however, be aware that
sanity checking is minimal (if at all):

* `users` *ideally you could configure autokey differently per user, and this
          nested list of users allows for that*
  * `username`
    * this is the username on the OS **NOTE: this role will not create the
      user!**
  * `repo`
    * this is a git repository containing autokey templates.  **NOTE: this must
      be accessible without input**

## Example Playbook

Playbook with various options specified:

```yaml
- hosts: localhost
  connection: local
  roles:
    - role: ansible-role-autokey
      users:
        - username: test_usr1
          repo: https://github.com/public/autokey.git
```

## Inclusion

I envision this role being included in a larger project through the use of a
`requirements.yml` file.  So here is an example of what you would need in your
file:

```yaml
# get the autokey role from github
- src: https://github.com/thisdwhitley/ansible-role-autokey.git
  scm: git
  name: autokey
```

Have the above in a `requirements.yml` file for your project would then allow
you to "install" it (prior to use in some sort of setup script?) with:

```bash
ansible-galaxy install -p ./roles -r requirements.yml
```

## Testing

I am relying heavily on the work by Jeff Geerling in using molecule for testing
this role.  I have, however, modified the tests to make them specific to what I
am attempting to accomplish but this could still use some work.

Please review those files, but here is a list of OSes currently being tested 
(using geerlingguy's container images):

* fedora27
* fedora28
* fedora29

## To-do

* N/A

## References

* [How I test Ansible configuration on 7 different OSes with Docker](https://www.jeffgeerling.com/blog/2018/how-i-test-ansible-configuration-on-7-different-oses-docker)

## License

All parts of this project are made available under the terms of the [MIT
License](LICENSE).
