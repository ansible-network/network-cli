# Parse
The ```parse``` task will take the output from a command that has been
previously executed on a remote network device and transform it into JSON data
that can be stored as facts for the host.

The ```parse``` task performs the parsing based on a set of rules defined in a
parser file that defines which values to extract from the output and how to
assign them to fact keys.  The parser language accepts a number of directives
that can be used to control how data is extracted and assigned to keys.  

## How to parse the output of a command
The basic function of this role is to pass the output of the show command
through a parser to transform the text into another format.  

```
---
- hosts: all

  tasks:
    - import_role:
        name: network-cli
        tasks_from: parse
      vars:
        parser: files/show_version.yaml
        contents: "{{ lookup('file', 'output/ios/show_version.txt') }}"
```

## How to use TextFSM as the parsing engine
The ```parse``` action also supports using the [TextFSM](https://github.com/google/textfsm)
engine as opposed to the default ```text_parser``` engine.  To use the TextFSM
engine set the [parser_engine](#parser_engine) variable to ```textfsm```.

For example:

```
---
- hosts: all

  tasks:
    - import_role:
        name: network-cli
        tasks_from: parse
      vars:
        parser: files/show_version.yaml
        contents: "{{ lookup('file', 'output/ios/show_version.txt') }}"
        parser_engine: textfsm
```


## Requirements
The following is the list of requirements for using the this task:

* Ansible 2.5 or later

## Operation

## Arguments
The following are the list of required and optional arguments supported by this
task.

### parser
The ```parser``` argument is required and defines the path to the parser file.
The parser file contains the set of rules used to parse the ```contents``` and
return JSON data.

### contents
The ```contents``` arugment is a required argument.  It provides the data that
is parsed in order to generate the JSON data based on the rules in the parser
file.

### parser_engine
The ```parser_engine``` variable is used to define which text parsing engine to
use when parsing the output of the CLI commands.  This action currently
supports two different pasrsers:

* text_paser (default)
* textfsm

The default value is ```text_parser```

## Notes
None

## Changelog

* 2.5.0 - initial task added to the role

