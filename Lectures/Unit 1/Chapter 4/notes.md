# Running Commands

## Creating a script

You can run regular commands, but if you wanna save the set of commands you
can use a script. You can put a set of commands into a file that ends in `.PS1`
which can be run

## Parts of a command

We will use a general cmdlet example `Get-Eventlog -LogName Security -ComputerName WIN8,SERVER1 -Verbose`

### Command

Every cmdlet needs to have a name! In this command this is called `Get-Eventlog`.

### Parameters

There are multiple parameters in this example. Each parameter is made up of the
name and a value. The name is there so we can provide `n` possible parameters, 
saying that there are also positional parameters which do not require a name.

In this example command we have the following parameters:

```
 -LogName 
 -ComputerName
 -Verbose
```

`LogName` is a regular parameter, since we are passing in the word `Security` 
which doesn't contain any spaces we don't need to use quotes.

`ComputerName` shows that you can pass multiple values to a parameter, again
separated by a comma and without quotes as there are no spaces.

`Verbose` is a boolean or switch parameter. It can either be true or false, 
which means you don't need to pass anything other than the parameter name.

## Naming Conventions

- `cmdlet` - Powershell command line utility which is written in another language 
    (C#) and are loaded into the shell.
- `function` is similar to a powershell cmdlet but are written in the powershell
    scripting language
- `workflow` - Function that ties into the powershell workflow system.

### How to name
All functions follow the following naming scheme:

`<Verb>-<Noun>`

For example: `New-Service`, `Get-Service`, `Set-Service`

## Aliases

Aliases are just custom names for cmdlets. So instead of 

`Get-Service`

You could just create an alias called `gsr`:

`Set-Alias gsr Get-Service`

And now anytime you wanna get services, you can simply call `gsr`.

There are also default system aliases which can be found by running `Get-Alias`

## Errors

You'll likely run into some errors when working with cmdlets. It will output 
a description of the error in red which you can use to debug.