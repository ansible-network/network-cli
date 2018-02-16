# Task run
The ```run``` task provides an implementaiton for running CLI commands on
network devices that is platform agnostic.  The ```run``` task accepts a
command and will attempt to execute that command on the remote device returning
the command ouput.  

If the ```parser``` argument is provided, the output from the command will be
passed through the parser and the contents returned as JSON data.  See
[parser](parser.md) for more details.

## Requirements
The following is the list of requirements for using the this task:

* Ansible 2.5 or later
* Connection ```network_cli```

## Arguments
The following are the list of required and optional arguments supported by this
task.

### command
This argument specified the command to be executed on the remote device.  The
```command``` argument is a required value.

### parser
The ```parser``` argument is an optional value that may be provided.  The value
must specify the location of the parser to pass the output from the command to
in order to generate JSON data.

## Notes
None

## Changelog

* 2.5.0 - initial task added to the role

