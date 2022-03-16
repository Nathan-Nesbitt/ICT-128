# Variables

- Variables are simply ways to store and reference information on the system

## Storing

- Creating a variable has the following syntax:

```ps
$my_variable = Get-Processes
```

- We define a name which is prefixed with a `$`
- We use the equals sign `=` to assign the variable `$my_variable` to the output
    of `Get-Process`
- Now if we call `$my_variable` it will output the results of our previous run 
    of `Get-Process` 

## Multiple Values (Arrays)

### Creation
- We can store multiple values in one variable (an array!)

```ps
$values = 'A','B','C'
```

- We have now created a variable called `$values` which contains 3 values.

### Indexing
- We can access these objects by their index:

```ps
$values[0]
$values[1]
$values[2]
```

This would output

```
A
B
C
```

- We can also index starting from the end, so we can access the last element in 
    the array by running

```ps
$value[-1]
```

which outputs

```
C
```

### Methods

- The safest way to call a method for each of the objects is to specify the 
    index, but it is also possible in powershell versions >= V3 to call a method
    on the array.

```ps
$values.ToLower()
```

which outputs

```
a
b
c
```

- You can also do the following if you want to iterate over the objects in the 
    array

```ps
$values | ForEach-Object { $_.ToLower()}
```

## Using in Strings

### Single Quotes

Single quotes are literals, which means they won't inject variables.

```ps
$variable = "Hello"
'$variable World'
```

Outputs:

```
$variable World
```

### Double Quotes

Double quotes interpret the variable and injects it into the string

```ps
$variable = "Hello"
"$variable World"
```

Outputs

```
Hello World
```

### Evaluating commands in strings

Sometimes we want to run powershell inside a string, we can do this by
wrapping the powershell code in `$()` inside the string:

```ps
$variable = 3, 2, 1
"The 1st value in the array is $($variable[0])"
```

### Escape Characters

We can escape special characters by using the escape character \`

```ps
$variable = "Hello"
"`$variable contains $variable"
```

```
$variable contains Hello
```

## Typing

We often want to indicate what type a variable should be. For example if we
are doing math, we don't want to treat our numbers like strings, we want to
treat them like integers or floats.

An example of this happening is when we use the `Read-Host` cmdlet:

```ps
$val = Read-Host "Number"
$val*$val
```

Which returns the completely wrong answer:

```
10101010101010101010
```

To solve this we can use typing:

```ps
[int]$val = Read-Host "Number"
$val*$val
```

Which fixes the type to be `int` instead of `string`

```
100
```

## Commands

- Although you don't really need any of these cmdlets to work with variables,
    they exist.

    - `New-Variable`
    - `Set-Variable`
    - `Get-Variable`
    - `Remove-Variable`
    - `Clear-Variable`

## Best Practices

- Don't use spaces (it's possible, you can wrap the variable in curly braces
    like `${My Variable}` but it's gross)
- Make sure your variables are named logically, don't use `$a` or `$foo` for
    anything serious as it becomes impossible to follow the code.