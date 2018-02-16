# Role Name: network-cli
This role provides tasks for running CLI commands on network devices.  It also
provides a basic parsing and templating langauge for parsing the output of
device commands and returned structured JSON data as host facts.

Any open bugs and/or feature requests are tracked in [Github issues](../../issues).

Interested in contributing to this role, please see [CONTRIBUTING](CONTRIBUTING.md)

## Requirements
None

## Role Tasks
The following are the available tasks provided by this role for use in
playbooks.

* run [[source]](tasks/run.yaml) [[docs]](docs/run.md)
* parse [[source]](tasks/parse.yaml) [[docs]](docs/parse.md)

## Role Variables
The following are the list of variables this role accepts

None

## Modules
The following is a list of modules that are provided by this role.

None

## Plugins
The following is a list of plugins that are provided by this role.

None

## Dependencies
The following is the list of dependencies on other roles this role requires.

None

## Example Playbook
To use the modules and plugins found in this role, simply include this role
in the desired playbook or add this role to another roles dependencies.

There are a set of predefined parsers available at http://github.com/ansible-network/parsers

```
vars:
  commands:
    eos:
      command: show interfaces | json
      parser: show_interfaces_json
    ios:
      command: show interfaces
      parser: show_interfaces

tasks:
  - name: run command and return parsed output
    include_role:
      name: network-cli
      tasks_from: run
    vars:
      parser: "{{ commands[ansible_network_os].parser }}"
      command: "{{ commands[ansible_network_os].command }}"

  - name: parse output and return facts
    include_role:
      name: network-cli
      tasks_from: parse
    vars:
      parser: show_interface
      contents: "{{ lookup('file', 'output/show_interface.txt') }}"
```

## License
GPLv3

## Author Information
Ansible Network Engineering Team
