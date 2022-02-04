# Pipelines

## How data is passed through the pipeline

- We can pipe information from one command into another

```ps
Get-Process -name note* | Stop-Process
```

- But how does powershell know which parameter to pass the result from the left 
    hand side into?
- This is called pipeline parameter binding
- We have 2 ways of handling this binding, either by value or property name.
- We always start by value then we go by property name

### By Value

- We can start by value, which essentially checks to see if the value matches one of the
    inputs to the command
- For the previous commands we can see how this is done

```ps
Get-Process | Get-Member
```

- This outputs `TypeName: System.Diagnostics.Process`
- Now we can check how the Stop-Process cmdlet works


```ps
help Stop-Process
```

- This outputs all of the params `Stop-Process` takes in.
- We can see that the first param that takes in process is `-InputObject <Process[]>`

### By Property Name

- If it fails to kill by value it will try by property name
- So for example if we want to run the following code

```ps
Get-Service -name s* | Stop-Process
```

- We are trying to pipe the name into `Stop-Process`, but there are multiple possible 
    inputs to `Stop-Process` that could satisfy the value type
- We can instead use the resulting values, for example `Name` which matches up with one
    of the parameters `-Name` for `Stop-Process`
- As long as `ByPropertyName` has been set this will work
- Saying this, the problem you will run into is that all of the names from
    `Get-Service` don't match up with the properties of `Stop-Process`
- You could re-run this example if you created a file called `names.csv` with the following CSV

```csv
Name, Value
d, Get-ChildItem
go, Invoke-Command
```

- Then you can pipe this into the `Get-Member` command

```ps
Import-CSV names.csv | Get-Member
```

## Custom Properties

- Let's say we wanna create some users using a file we have been given
- We cannot do a straight pipe as the titles don't match
- Lets create a CSV file `computers.csv` with the following
```
username,dept,city,title
nate,tech,kamloops,SDE
```

- Now how can we do this? Easy!


```ps
import-csv .\computers.csv | select-object -property *, 
    @{name='samAccountName';expression={$_.username}},
    @{label='Name';expression={$_.username}},
    @{n='Department';e={$_.Dept}}
```

- This adds new columns to the data with some new titles.
- This way we can re-format the data to match the expected input for the next cmdlet

```ps
import-csv .\computers.csv | select-object -property *, 
    @{name='samAccountName';expression={$_.username}},
    @{label='Name';expression={$_.username}},
    @{n='Department';e={$_.Dept}} | 
    new-aduser
```

## Parenthetical Commands

- Sometimes you just can't make pipeline input work
- Let's create a file called `computers` and put the following in

```
WIN8
NATECOOLPC
```
- Now let's say we want to get the list of computers and pipe them into the `Get-WmiObject`
    command.  
- We can't just pipe it in, the `Get-WmiObject -ComputerName` param doesn't allow pipeline 
    input
- Well instead we can wrap commands with parentheses to get them to run first and treat 
    them like raw input to the command

```ps
Get-WmiObject -ComputerName (Get-Content .\computers.txt)
```

## Extracting a value from a single property

- Now what if we just want to use one of the properties from a CSV file or other object?
- Let's create a CSV file called `processes.csv` with the following

```
Name, Id

```

```ps
Get-Process (import-csv .\processes.csv | select -expand Id)
```

- We can just leverage the `select` cmdlet within a bracket to ensure only one
    of the values is extracted and returned.