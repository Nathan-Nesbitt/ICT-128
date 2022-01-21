# Pipelines

## Understanding pipelines
- The basic idea is that the result from the leftmost command is passed
    into the next command. 
- This can be chained, so the output of each command is then sent into the next
- We can take specific datatypes outputted from commands and pipe them into
    other commands

Example:

`"hello" | Out-File foo.txt`

## Exporting input to a specific format

- There are many different ways to store and represent data
    - CSV
    - XML
    - HTML

All of these have their own cmdlet which takes some input, and outputs it
as the specified format.


## Exporting to printers

- This works very similarly to the rest of the `Out` commands
- The command can only be run on windows `Out-Printer`

## Killing Processes

## Functions

- Sidebar about functions
- Since we may want to bundle a bunch of code together into one nice logical chunk