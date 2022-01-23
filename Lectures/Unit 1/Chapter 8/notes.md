# Objects

- Objects are useful in programming and scripting as programming mimicks
    what we see in real life
- Being able to bundle related data together will make our lives easier

## Components of objects

### Property

- A property is a non-runnable value related to the object.
- For example, if our object was a book we could have the following properties
    - title
    - author
    - subject
    - pages

- For example, we can access the properties of the processes on the system (ID)
    using the following syntax
    
```ps
$process = Get-Process
$process.Id
```

### Method

- A method is a function that is associated with an object
- For example, if our object was a shower we could have the following methods:
    - start_shower
    - stop_shower
    - set_temperature

- We can access these propeties through variables. For example, if I wanted to
    get the first process from the list of processes I could use the `Item` method.

```ps
$process = Get-Process
$process.Item(1)
```

## Understanding an object

- If you're given a random output and you wanna know what the object is
    you can use the `Get-Member` command (`Gm` is the alias)
- This will give you a breakdown of the object that is outputted from the
    inputted command.

Example: `Get-Processes | gm`

- This outputs a list of `Name` and `MemberType` which define the name 
    and type for each of the properties in the object.

## Sorting objects

- Some objects don't have a default way of sorting so we can use the `Sort-Object` cmdlet

- You can pass in one of the properties of the object, for example the
    `ID` of the process, and Sort-Object will sort on it.

Example: `Get-Process | Sort-Object ID -desc`


## Selecting properties

- Sometimes we just want a couple of properties of an object.
- We can use `Select-Object` to only select certain properties of objects

Example: `Get-Process | Select-Object -property Name,ID`

## Pipeline of Objects

- Until we print, everything is an object.
- This means that every time you pipe a command into another command, the
    type of the object can change.