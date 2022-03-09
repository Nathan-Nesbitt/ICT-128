# Creating Scripts

- We have already covered a bit in class on how to create scripts but here are some best
    practices

## Storing in a file

- You have created regular script files `.ps1`
- Scripts work by clustering all of the commands that we want into one file
    that can be run as a whole.
- For example if we want to have an easy to run script that asks for a user's
    name then prints out `hello <name>` we could create a `.ps1` file with the 
    following

```ps
$name = Read-Host "What is your name"
echo "hellp $name"
```

- Most of the labs in this course have been done using modules `.psm1`
- Modules allow us to import individual functions into our environment to be run

## Adding Parameters

- We have done this before in the labs, but you can use this without a function which
    works equally as well
- If you want to accept parameters to a powershell script you can use the `param` call
    which will accept parameters to the script
- For example, if we wanted to accept a name and age for our script we could create a
    `.ps1` file with the following code:

```ps
param(
    [string] $name,
    [int] $age
)

echo "Hello $name, you're $age years old!"
```

- We can then call this script using `<script name>.ps1 <name> <age>` 
- for example `test.ps1 nathan 10` would output `Hello nathan, you're 10 years old!`

## Adding Help

- We should always help out the people who are using our scripts by providing help files.
- We can create the help file for our script by simply adding in a comment above our script
- The following is a simple demo help file 

```ps
<#
.SYNOPSIS
A simple program that displays someones name and age!
.DESCRIPTION
This program takes in someones name and age, then makes a statement
about their age and name.
.PARAMETER name
The name of the person running the script! 
.PARAMETER age
The age of the person running the script!
.EXAMPLE
test.ps1 nathan 10
#>

param(
    [string] $name,
    [int] $age
)

echo "Hello $name, you're $age years old!"
```

- You can then access this help file by running `help ./<filename>.ps1`
- Each of the `.<NAME>` sections are keywords that indicate to the help function
    what to display. There are many to choose from.

## Script Pipelines

- When you run a script, it uses one pipeline
- This means it may run differently than if you run each command line-by-line from the CLI
- This means that the output will be formatted specifically to the first object that is 
    returned
- For example, if we run the following script in a file it will produce different results
    than if we run it from the CLI.

```ps
Get-Process
Get-Service
```

## Scope

- Scripts have their own scope
- This means that any variables that we create within the scope are not accessible
    from outside of the scope
- This also applies to functions. 
- This is useful as we don't always want our variables to be "global" which means
    accessible from anywhere.