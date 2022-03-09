# Loops

## For Loops

- Most common loop
- Useful for iterating over data with a defined length
- For example, if you want to repeat a task `n` times.

```ps
$n = 10 # We want to run 10 times
for($i = 0; $i -lt $n; $i++) {
    Write-Host "$i"
}
```

- The for loop is made up of 3 parts:
    - An initializer `$i = 0`
    - A condition `$i -lt $n`
    - A modifier `$i++`

- This results in the following steps:
    1.  When we create the loop, the initializer is run creating
    a variable called `$i`
    
    2. The loop then checks the condition `$i -lt $n` (is $i 
    less than $n). If this is true, the code inside the brackets
    is run. If this is false, we break and continue.

    3. Once the code in the brackets has been run, the modifier 
    is executed `$i++`. This moves our counter `$i` one closer
    to the condition's breaking point.
    
    4. Go back to step 2.

- We can also loop over a dataset using its length
- For example if we want to print out the ID of a list of 
    processes we can achieve this using a for loop

```ps
$processes = Get-Process
$n = $processes.Count
for($i = 0; $i -lt $n; $i++) {
    $processID = $processes[$i].Id
    Write-Host "$processID"
}
```

- **Be careful** you need to ensure that your modifier moves
    the initialized variable towards the end of the condition.
    If you do the following, you will end up in an infinite loop.

```ps
$n = 10 # We want to run 10 times
for($i = 0; $i -lt $n; $i--) {
    Write-Host "$i"
}
```

- You should also note that the operand `-lt` has been used
    in these examples. If you need to start at 1 and end on
    the value specified by `$n` you can do the following:

```ps
$n = 10 # We want to run 10 times
for($i = 1; $i -le $n; $i++) {
    Write-Host "$i"
}
```

## For Each Loops

- If we have a list of objects, and we don't care about the
    index of the object we can also use a foreach

```ps
$processes = Get-Process
forEach($process in $processes) {
    $processID = $process.Id
    Write-Host "$processID"
}
```

## While Loops

- While loops are more useful when we do not know how many 
    iterations we are going to be performing but we do know
    a condition that will stop the loop.

- You'll often see this when you want to recieve infinite input
    from a user

- An example of a while loop which counts from 0-9

```ps
$i = 0
while($i -lt 10){
    Write-Host $i
    $i++
}
```

- Or if you want to read in input until someone enters `Q`

```ps
while(($input = Read-Host "User Input") -ne "Q"){
    Write-Host $input
}
```

- **Be aware**, like for loops you need to have a break case
    that is reachable or you will have an infinite loop. For 
    example the following will run infinitely:

```ps
$i = 0
while($i -lt 10){
    Write-Host $i
    $i--
}
```    

## Do While

- Similar to the while loop, the do while simply runs the code
    before checking the condition.

```ps
do{
   $input = Read-Host "User Input"
}
while ($input -ne "Q")
```


## Break

- You can also insert a break which breaks out of a loop early

```ps
$i = 0
while($i -lt 10){
    if($i -eq 5) {
        break;
    }
    Write-Host $i
    $i++
}
```