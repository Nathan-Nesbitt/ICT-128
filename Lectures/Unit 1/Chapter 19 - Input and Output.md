# Input and Output

## Read

- We can read from the standard input for powershell using `Read-Host`
- `Read-Host` takes in a string which is the prompt

```ps
$name = Read-Host "Enter your name"
$name
```

- If you are working in ISE this will create a popup

## Write

- We can write to powershell using the `Write-Host` cmdlet
- The `Write-Host` cmdlet takes in more parameters than `Read-Host` which you 
    can check out using `help Write-Host`

```ps
Write-Host "Hello World"
Write-Host "Hello World" -BackgroundColor blue
Write-Host "Hello World" -ForegroundColor yellow
``` 

## Pipeline

- For anything other than printing text or getting input from the user we want
    to use `Write-Output`

- This is a better approach as it puts the text into the pipeline which allows
    you to run commands on it before it's outputted to the screen.

## Errors and Warnings

- We can also output text in different ways so we can control how they are displayed.

- For example, if we want to supress warnings, by using the righ cmdlets for
    output it is an easy task. 

- There are multiple different ways of displaying this information:

    - `Write-Warning`
    - `Write-Verbose`
    - `Write-Debug`
    - `Write-Error`
 