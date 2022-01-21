# Pipelines

## Understanding pipelines
- The basic idea is that the result from the leftmost command is passed
    into the next command. 
- This can be chained, so the output of each command is then sent into the next
- We can take specific data-types outputted from commands and pipe them into
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

- `Export-CSV` and `Import-CSV`
- `Export-CliXML` and `Import-CliXML`
- `ConvertTo-HTML`

Some examples:

`Get-Process | Export-CSV processes.csv`
`Get-Process | Export-CliXML processes.xml`
`Get-Service | ConvertTo-HTML | Out-File processes.html`

## Exporting to printers

- This works very similarly to the rest of the `Out` commands
- The command can only be run on windows `Out-Printer`

## Killing Processes

- We can manage processes using the CLI as well
- The command for getting the processes is `Get-Process` or `ps`
- We can kill a process using `Stop-Process`

Example: `Stop-Process -Id 13248` kills the process with ID 13248

## Functions

### What are functions
- Sidebar about functions
- Since we may want to bundle a bunch of code together into one nice logical chunk

### Creating a function

```ps
function MyFunction {
    echo "Bonjour"
}
```

### Running a function

```ps
MyFunction
```

### Returns
- Normally a function returns a value
- Anything that gets outputted during the function gets returned.
- We can explicitly return values using the `return` keyword

Example:

```ps
function RandomValue {
    return Get-Random
}
```

## Conditions

- We need to be able to control the flow of information in our scripts
- To do this we use three different terms `if`, `elif` and `else`.

### IF

- If allows us to check if something is true. If it is true, we run the code in
    the brackets.

```ps
if($true) {
    echo "TRUE!"
}
```

### ELIF

- If allows us to check if something is true. If it is true, we run the code in
    the brackets.

```ps
if($false) {
    echo "This never runs!"
} elif ($true) {
    echo "This runs tho!"
}
```

### ELSE

- We have else which runs if none of the previous expressions were true

- If allows us to check if something is true. If it is true, we run the code in
    the brackets.

```ps
if($false) {
    echo "This never runs!"
} elif ($false) {
    echo "This never runs either!"
} else {
    echo "This runs tho!"
}
```