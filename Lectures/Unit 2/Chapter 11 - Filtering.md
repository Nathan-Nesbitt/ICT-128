# Filtering

- Filtering can be achieved in 2 ways: Early or Late Filtering
- Normally we want to use early filtering as it reduces the computational cost 
- Sometimes it's not possible to use early filtering, so we need to use comparisons.
- We always want to keep the 

## Early Filtering

- Early filtering is when we limit the number of parameters within the cmdlet that we are calling
- An example of this is when we filter the number of files that are displayed when we run `ls`
- For example, we could only get files that start with a

```ps
ls a*
```

- Or another example, we could get services that contain the letter c or e in the name

```ps
Get-Service -name *c*, *e* 
```

## Comparison Operators

- Comparison operators can be used to check the relationship between two values
- The result of a comparison is either `true` or `false`
- Here is a list of some comparison operators

```ps
-eq # Check if equal
-ne # Check if not equal
-gt # Check if left is greater than right
-lt # Check if left is less than right
-ge # Check if left is greater than or equal to right
-le # Check if left is less than or equal to right
-not # Check if value is not true
```
-  We can also prepend `c` to any of these for comparing strings to assert that the cases match as well

## Removing Objects From the Pipeline (Late Filtering)

- This process involves iterating over the data once is has left the initial cmdlet
- We can do this using the `Where-Object` cmdlet also known as `Where`
- For example if we wanted to get all the services, and filter on the services that are running we could write the following

```ps
Get-Service | Where-Object -filter { $_.Status -eq 'Running' }
```

- Like before, we are using the `$_` to indicate each value in the output. We are then checking to see if the `Status` property for each 
    service is eqivalent to `"Running"`

## How to approach filtering

- Essentially always start small when filtering, don't try to write one big long command at once
- Let's say we have been given the following instruction: "Calculate the sum of the top five virtual memory processes on the system"
- How do we solve this? Break it down into steps

1. Find processes `Get-Process`
2. Remove any process that is associated with powershell (because that's kinda redundant) `Get-Process | Where-Object -filter { $_.Name -notlike 'powershell*' }`
3. Sort the processes `Get-Process | Where-Object -filter { $_.Name -notlike 'powershell*' } | Sort VM -descending`
4. Filter out all but the top five processes `Get-Process | Where-Object -filter { $_.Name -notlike 'powershell*' } | Sort VM -descending | Select -first 5`
5. Sum the memory of these processes `Get-Process | Where-Object -filter { $_.Name -notlike 'powershell*' } | Sort VM -descending | Select -first 5 | Measure-Object -property VM -sum`
