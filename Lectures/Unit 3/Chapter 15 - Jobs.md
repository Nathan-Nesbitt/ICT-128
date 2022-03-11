# Jobs

- Most of the scripts we've written have been performed in the 
    foreground

- Now sometimes when we have processes that take a long time,
    or we don't want to show to users.

- We can create Jobs to solve this problem.

## Synchronous vs Asnychronous

- `synchronous` is when we run line by line, top to bottom, until
    the script is done.

- `asychronous` means that we can have multiple processes running at the same time. 

- Jobs are run asychronously which is beneficial because
    - Frees up the UI/terminal for more commands
    - ALlows you to run multiple computing problems at the same 
        time.
- The downsides are that
    - They do not take input from stdin
    - They log errors to the background
    - Output will not be displayed until queried

## Local Jobs

- You can create a local job using `Start-Job`
- For example, a simple script that runs a command in the 
    background can be achieved by running

```ps
Start-Job -scriptblock { 
    # YOUR SCRIPT HERE # 
}
```

- So for example, if we wanted to run a script that finds the sum
    of numbers from 1 to 10000 we could write

```ps
Start-Job -scriptblock { 
    $result = 1;
    for($i = 2; $i -le 10000; $i++) {
        $result += $i;
    }
    Write-Host $result
}
```
## Job Results

- When our job is complete we will sometimes want to see the results
- We can view a list of jobs on the system by running

```ps
Get-Job
```

- We can then use the `id` of the job to fetch the data

```ps
Recieve-Job -id <YOUR ID>
```

- If you want to keep the results in memory, you can include
    the `-keep` parameter. If you don't use keep, once you
    recieve the job, it will pull all data from memory.

- We can also query the job based on the name

```ps
Receive-Job -name <JOB NAME>
```

## Child Jobs

- Jobs can also have children
- For example, we can call start-job, with a start-job within
    it.

```ps
Start-Job -scriptblock { 
    Start-Job -scriptblock { 
        # YOUR SCRIPT HERE # 
    }
}
```

- If you want to see the child-jobs of a job you can simply
    query the job and parse it using `Select-Object`

```ps
Get-Job -id 1 | select-object -expand childjobs
```

## Managing Jobs

- There are some more commands that are useful when working with
    jobs
    
    - `Remove-Job` - Removes a job and associated output from 
        memory
    - `Stop-Job` - Kills a job
    - `Wait-Job` - Waits for a job to complete, then continues

- For example, if you want to remove any job that doesn't have 
    any more data you could run

```ps
Get-Job | Where { -not $_.HasMoreData } | Remove-Job
```

## Scheduled Jobs

- We can also schedule jobs at some time in the future, or on a 
    repeated basis.

```ps
Register-ScheduledJob `
    -Name MyJob `
    -ScriptBlock { Get-Process } `
    -Trigger (New-JobTrigger -Daily -At 12am) `
    -ScheduledJobOption (New-ScheduledJobOption -WakeToRun)
```

- This creates a job that will run daily at 12am and gets a list
    of the processes on the system. It will wake the system to
    run the script.

- We can get the scheduled jobs by running `Get-ScheduledJob`

## Remote Jobs

### WMI

- You can run WMI as a job by passing the `-asjob` parameter

```ps
Get-Wmiobject win32_operatingsystem -computername <COMPUTERNAME> –asjob
```

- For example we could do this on our local machine

```ps
Get-Wmiobject win32_operatingsystem -computername localhost –asjob

```

### Invoke-Command

- We can also run jobs remotely using `Invoke-Command`
- In our demo we will use a VM but you can use a remote machine

```ps
invoke-command `
    -Command { Get-Process } `
    -VMName <VMNAME> # IF YOU ARE USING A COMPUTER USE ComputerName`
    -Asjob
```