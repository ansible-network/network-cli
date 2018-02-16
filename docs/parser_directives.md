CLI Parser Directives
=====================
The ```cli_facts``` module is a module that can be used to parse the results of
text strings into Ansible facts.  The primary motivation for developing the
```cli_facts``` module is to convert structured ASCII text output (such as
those returned from network devices) into structured JSON data structures
sutable to be used as host facts.

The parser file format is loosely based on the Ansible playbook directives
language.  It uses the Ansible directive language to ease the transition from
writing playbooks to writing parsers.  Below is the set of supported directives
the ```cli_facts``` module recognizes along with a description of the
directives basic functionality.

# name
All entries in the parser file many contain a ```name``` directive.  The
```name``` directive can be used to provide an arbitrary description as to the
purpose of the parser items.

# context
The purpose of the ```context``` directive is to filter the text into sub
itemss that are iterated over for parsing facts.  The ```context``` directive
defines a ```start``` and ```end``` value that is used to identify the items.

All ```context``` data is extracted from the overall text items and then
iterated on looking for ```matches```.  For example, take the following output
from a command:

```
Ethernet1 is up, line protocol is up (connected)
  Hardware is Ethernet, address is 0050.5694.d353 (bia 0050.5694.d353)
  Ethernet MTU 9214 bytes
  Full-duplex, Unconfigured, auto negotiation: off, uni-link: n/a
  Up 12 days, 11 hours, 1 minutes, 3 seconds
  Loopback Mode : None
  1 link status changes since last clear
  Last clearing of "show interface" counters never
  5 minutes input rate 0 bps (- with framing overhead), 0 packets/sec
  5 minutes output rate 0 bps (- with framing overhead), 0 packets/sec
     35884 packets input, 7320093 bytes
     Received 0 broadcasts, 35884 multicast
     0 runts, 0 giants
     0 input errors, 0 CRC, 0 alignment, 0 symbol, 0 input discards
     0 PAUSE input
     570108 packets output, 73029816 bytes
     Sent 0 broadcasts, 570108 multicast
     0 output errors, 0 collisions
     0 late collision, 0 deferred, 0 output discards
     0 PAUSE output
Ethernet2 is up, line protocol is up (connected)
  Hardware is Ethernet, address is 0050.5694.8fd4 (bia 0050.5694.8fd4)
  Ethernet MTU 9214 bytes
  Full-duplex, Unconfigured, auto negotiation: off, uni-link: n/a
  Up 12 days, 11 hours, 1 minutes, 3 seconds
  Loopback Mode : None
  1 link status changes since last clear
  Last clearing of "show interface" counters never
  5 minutes input rate 0 bps (- with framing overhead), 0 packets/sec
  5 minutes output rate 0 bps (- with framing overhead), 0 packets/sec
     568101 packets input, 72782865 bytes
     Received 0 broadcasts, 568101 multicast
     0 runts, 0 giants
     0 input errors, 0 CRC, 0 alignment, 0 symbol, 0 input discards
     0 PAUSE input
     35889 packets output, 7320879 bytes
     Sent 0 broadcasts, 35889 multicast
     0 output errors, 0 collisions
     0 late collision, 0 deferred, 0 output discards
     0 PAUSE output
```

The above text could be split into logical contexts for the purposes of
performing regular expression matching.  In order to split the text, the sub
directives ```start``` and ```end``` must be defined.

## start
The ```start``` directive identifies the beginning of a context.  This
directive accepts a valid regular expression used to identify the begging of
the context.  

## end
Just as the ```start``` directive, the ```end``` directive accepts a valid
regular expression and indentifies the end of the context items to iterate on.
The ```end``` regular expression is identified as the first match after the
```start``` expression is matched.

To use the ```context``` directive, both the ```start``` and ```end``` sub
options are required.  Using the example text above, the directives could be
used to split the output, for matching purposes, into separate sections using
the following:

```
context:
  start: ^Ethernet\\d+
  end: \d+ PAUSE output
```

The regular expressions for the context will split the string and then iterate
over each context, attempting to find values based on regulare expressions as
defined in the ```matches``` directive.

# items
Using the ```items``` directive, groups of statements can be combined for
looping over a set of data.  The ```items``` directive, when used, will apply
the looped items to each member of the items.  

# facts
The ```facts``` directive identify the set of facts that should be defined by
the parser.  The ```facts``` directive accepts a nested dictionary structure
that will be returned from the parser with assoicated variable substititued.

## key
The ```key``` directive will set the fact key name in the returned facts.

## value
The ```value``` directive will set the item value for the key in the returned
facts.

# matches
The ```matches``` directive define one or more matching objects that are used
to extract values from the text.  Each match object is defined using one or
more of the following match directives.

## pattern
This directive defines a regular expression pattern that is used to extract one
or more matching values from the text.  This directive accepts any valid
regular expression or keyword value.

Keyword values are predefined match objects that can be used to simply the
construction of a regular expresssion.  For example, lets assume the pattern
needs to match a valid IPv4 address, then the pattern could be constructed as 
follows:

```
matches:
  - pattern: "ip address {{ IPV4 }}"
```

The value for IPV4ADDR will be replaced with a valid regular expression that
will extract the IPv4 address from the text.

## match_var
The ```match_var``` directive is used to hold the contents of the matched
values from the ```pattern``` directive.  The ```match_var``` directive
simplifies variable substititution for facts.  

The contents of the pattern are also accessible from the ```matches```
variable if the ```match_var``` directive is not used.

## match_all
The ```match_all``` directive is used to find all occurances of a value in the
text.  The ```match_all``` directive accepts a boolean value of True or False.

The default value for ```match_all` is False.

# loop
The ```loop``` directive instructs the ```facts``` directive to iterate over
the set of values identified by ```loop```.  This is useful when parsing values
that should return a list of items.


