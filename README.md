ansible-role-dsu
================

DELL System Update (DSU) repo setup and package installation

Description
------------

It is an implementation of the script 

```
curl -O https://linux.dell.com/repo/hardware/dsu/bootstrap.cgi
sed -i 's/IMPORT_GPG_CONFIRMATION="na"/IMPORT_GPG_CONFIRMATION="yes"/' bootstrap.cgi
bash bootstrap.cgi
yum -y update dell-system-update
```

but for Ansible in YAML syntax.

GPG keys are installed on the fly.

The rest is supposed to be done manually on demand, for instance

```
client host# dsu -n --import-public-key
client host# scontrol reboot ASAP nextstate=resume reason="apply DSU updates" $(hostname -s)
```

We intentionally do not diff the newly downloaded file against the current one (if any) but
rather force to proceed to make sure GPG keys and package are up-to-date always.

Example Playbook
----------------
```
vars:
  # redefine to True if you want to enable Dell hardware check
  dsu_assert: False

  # that is the default, no need to define at all if it is still there
  dsu_url: https://linux.dell.com/repo/hardware/dsu/bootstrap.cgi

  # the downloaded file will go to...
  dsu_bootstrap_local_copy: "/root/bootstrap.cgi"

  # clean up downloaded bootstrap file (Default is True)
  dsu_cleanup: True

roles:
  - { role: ansible-role-dsu, tags: dsu }
```

License
-------

GPLv2

Author Information
------------------

Aalto Scientific Computing Team
