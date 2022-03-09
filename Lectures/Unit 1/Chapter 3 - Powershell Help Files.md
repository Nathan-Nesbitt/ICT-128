# Powershell Help Files

## How to use `help`

Simple to use command, in your powershell terminal run the command 
`help <cmdlet>`

Example: `help Get-Service`

## How to update help

`update-help`

## `Get-Help` vs `help` vs `man`

`Get-Help` is the actual cmdlet, where `help` and `man` are aliases 
of the `Get-Help` cmdlet.

## Using help to find commands

`Get-Help -Name` or just `Get-Help` as name is a positional parameter.

Example: `Get-Help log` or `Help log`

## Accessing online help

Simply pass the `-online` parameter to the `help` function.