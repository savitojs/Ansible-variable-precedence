# Ansible-variable-precedence

Here is the order of precedence from least to greatest (the last listed variables winning prioritization):

- command line values (eg “-u user”)
- role defaults [1]
- inventory file or script group vars [2]
- inventory group_vars/all [3]
- playbook group_vars/all [3]
- inventory group_vars/* [3]
- playbook group_vars/* [3]
- inventory file or script host vars [2]
- inventory host_vars/* [3]
- playbook host_vars/* [3]
- host facts / cached set_facts [4]
- play vars
- play vars_prompt
- play vars_files
- role vars (defined in role/vars/main.yml)
- block vars (only for tasks in block)
- task vars (only for the task)
- include_vars
- set_facts / registered vars
- role (and include_role) params
- include params
- extra vars (always win precedence)


> Basically, anything that goes into “role defaults” (the defaults folder inside the role) is the most malleable and easily overridden. Anything in the vars directory of the role overrides previous versions of that variable in namespace. The idea here to follow is that the more explicit you get in scope, the more precedence it takes with command line -e extra vars always winning. Host and/or inventory variables can win over role defaults, but not explicit includes like the vars directory or an include_vars task.

**Footnotes**

> [1] Tasks in each role will see their own role’s defaults. Tasks defined outside of a role will see the last role’s defaults.

>	[2] (1, 2) Variables defined in inventory file or provided by dynamic inventory.

>	[3] (1, 2, 3, 4, 5, 6) Includes vars added by ‘vars plugins’ as well as host_vars and group_vars which are added by the default vars plugin shipped with Ansible.

> [4] When created with set_facts’s cacheable option, variables will have the high precedence in the play, but will be the same as a host facts precedence when they come from the cache.

[More reading](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable)
