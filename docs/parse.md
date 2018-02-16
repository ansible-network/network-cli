# Task parser
The ```parser``` task will take the output from a command that has been
previously executed on a remote network device and transform it into JSON data
that can be stored as facts for the host.

The ```parser``` task performs the parsing based on a set of rules defined in a
parser file that defines which values to extract from the output and how to
assign them to fact keys.  The parser language accepts a number of directives
that can be used to control how data is extracted and assigned to keys.  

The parser directives are documented [here](parser_directives.md)

## Requirements
The following is the list of requirements for using the this task:

None

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

## Notes
None

## Changelog

* 2.5.0 - initial task added to the role

