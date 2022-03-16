# Adding more to scripts

## Advanced Scripts

- Sometimes we want to turn our script into an advanced script
- This provides some extra functionality for our script

```ps
# This is where our comment based HELP info would go
[CmdletBinding()]

# This is where the rest of our script will go!
```

## Mandatory Parameters

- Since it is sometimes useful to be able to require that someone inputs
    a parameter we can now add in `[Parameter(Mandatory=$True)]` before
    any input to have the system prompt the user for input if it is
    not provided in the original command call

```ps
[CmdletBinding()]
param (
    [Parameter(Mandatory=$True)] [string] $name,
    [int] $age = 10
)

Write-Host "Hey $name, you are $age years old!"
```

- Technically as long as there is a comma `,` between each of the params
    you can split the `[Parameter(Mandatory=$True)]` onto a separate line

```ps
param (
    [Parameter(Mandatory=$True)] 
    [string] $name,
    [int] $age = 10
)
```

- Personally I think this is harder to read (but it is up to you)

## Parameter Aliases

- We like to keep the names consistent across all cmdlets, for example
    `computerName` is normally how cmdlets take in the name of the 
    computer
- But lets say you don't like this parameter name, and you want to change
    it to be a different parameter name

- The best way of handling this is to keep the parameter name the same,
    but create an alias for the parameter. 

- For example, let's say the standard parameter name for a user's name is
    `UserFullName`

- We decide that instead of writing `./script.ps1 -UserFullName nathan` we would
    rather use the parameter name `name` so the call looks like
    `./script.ps1 -name nathan`

- So let's add an alias for `UserFullName`!

```ps
[CmdletBinding()]

param (
    [Parameter(Mandatory = $True)] [Alias('name')] [string] $UserFullName,
    [int] $age = 10
)

Write-Host "Hey $UserFullName, you are $age years old!"
```

We can now call this script in 2 ways:

```ps
./script.ps1 -name nathan
## OR
./script.ps1 -UserFullName nathan
```

## Parameter Validation

- A good script should also do some checking to make sure the input is valid
- For example, let's say we are accepting an age to our script, and we know
    that no one will be entering an age that is less than 0 or greater than
    120 we could add in some validation to ensure that any other value will
    throw an error!

```ps
[CmdletBinding()]

param (
    [Parameter(Mandatory = $True)] [Alias('name')] [string] $UserFullName,
    [ValidateRange(0,120)] [int] $age = 10 
)

Write-Host "Hey $UserFullName, you are $age years old!"
```

- This will work for any value between 0 and 120 but will throw the following
    error for any other values:

```
 Cannot validate argument on parameter 'age'. The 121 argument is greater than the maximum allowed range of 120. Supply an argument that is less than or equal to 120 and then try the command again.
```

- If you have a set of values that must be chosen from, you can instead use the
    `ValidateSet()`.


## Verbose Output

- Sometimes we want to provide a more detailed output than what normally
    gets outputted
- For example, if we wanted to detail each step in a script, but only if
    the user specifies that they want that information we can use `Write-Verbose`

```ps
[CmdletBinding()]

param (
    [Parameter(Mandatory = $True)] [Alias('name')] [string] $UserFullName,
    [ValidateRange(0,120)] [int] $age = 10 
)

Write-Verbose "Reading in parameters"
Write-Verbose "Doing some crazy magical stuff"

Write-Host "Hey $UserFullName, you are $age years old!"
Write-Verbose "Script is complete!"
```

- You can run this script normally `./script.ps1 -name nathan` and get
    the following output

```
Hey nathan, you are 10 years old!
```

- OR you can run it with the `-verbose` flag which will print out all of
    the extra comments you added in (`./script.ps1 -name nathan -verbose`):

```
VERBOSE: Reading in parameters
VERBOSE: Doing some crazy magical stuff
Hey nathan, you are 0 years old!
VERBOSE: Script is complete!
```