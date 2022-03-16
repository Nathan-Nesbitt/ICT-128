# Remote Control

- We often want to connect to remote machines using powershell

- Instead of using SSH or Telnet you can use WS-MAN (Web Services for Management)

- WS-MAN works entirely over HTTP/HTTPS which means you don't need to
    worry about firewall rules.

- It's great because instead of just sending text, it will return the
    raw XML for the objects to your machine which allows you more control
    in terms of data manipulation.

- For all of the following examples, we will remote connect to a single
    VM. You can always replace `-VMName` with `-ComputerName` if you are
    not using VMs. In the demos the VM is normally named `Windows` just
    to keep it simple. 

## WinRM

- To be able to remote into a machine, the machine must be running WinRM

- WinRM (windows remote management) is an implementation of WS-MAN

- To start up WinRM we can run 

```ps
Enable-PSRemoting
```

- There is an alternative cmdlet called `Set-WSManQuickConfig` which is 
    called by `Enable-PSRemoting`

- If you run into an error on this step it's possible that you are 
    connected to a public network which does not allow firewall 
    exceptions.


## One-To-One Remoting

- One-to-one remoting means we are connecting to only one machine from
    one other machine

- You run the commands remotely and the results are returned locally

### Enter-PSSession

- We can create a connection to a PSSession by running `Enter-PSSession`

- If you know the computer name you can use the `-ConputerName` parameter
    to specify which device you want to connect to

```ps
Enter-PSSession -computerName MyCoolServer
```

- If you are using a VM like in the labs you can instead use `-VMName`

```ps
Enter-PSSession -VMName MyCoolVM
```

### Exit-PSSession

- Although it's fun to play on remote machines, normally we want to be
    able to leave them

- The `exit-pssession` cmdlet allows us to leave a session on a remote 
    machine. 

```ps
Exit-PSSession
```

- You can also just use ctrl-c/ctrl-d on many machines, but if you use
    the cmdlet in a script you will need to call a cmdlet to quit

## One-To-Many Remoting

- If you want to run the same command on many machines instead of just
    one then you can instead do one-to-many remoting.

### Invoke-Command

- `Invoke-Command` is the alternative to Enter-PSSession for multiple 
    machines except you are just running a single command not multiple.

```ps
Invoke-Command `
    -VMName Windows `
    -command { `
        Get-EventLog Security -newest 200 `
        | Where { $_.EventID -eq 1212 } `
    }
```

- We can break this command down into some basic parts:

    1. `Invoke-Command` - Simply the cmdlet call to run code on a remote 
        machine 
    2. `-VMName VM1, VM2` - Which machines to
        run the cmdlet on
    3. `-command {# Your Code Here }` - Command you want to run on the
        remote machine.

## Local vs. Remove Commands

### -computerName vs. Invoke-Command

- Let's consider the following two commands

```ps
Invoke-Command -VMName Windows `
    -command { Get-EventLog Security -newest 200 | `
    Where { $_.EventID -eq 1212 }}
```

```ps
Get-EventLog Security -newest 200 `
    -VMName Windows `
    | Where { $_.EventID -eq 1212 }
```

- These two perform the same operation on a set of remote machines: 
    it gets the newest 200 security logs where the event ID is 1212

- What is the difference then? Well the second command

    1. Performs the operations sequentially (slower)
    2. Doesn't include the `PSComputerName` property (we don't know 
        which computer this log is from)
    3. Doesn't use WinRM
    4. Gets all the records, sends them to our computer then we filter

- Normally it's better to use `Invoke-Command` instead of the 
    `computerName` parameter on commands.

### Processing

- Let's consider local vs remote processing

- We can use our original example, and compare it to a local approach

```ps
Invoke-Command -VMName Windows `
    -command { Get-EventLog Security -newest 200 | `
    Where { $_.EventID -eq 1212 }} 
```

```ps
Invoke-Command -VMName Windows `
    -command { Get-EventLog Security -newest 200 } | `
    Where { $_.EventID -eq 1212 }
```

- You're likely wondering the difference, to do this remotely we
    remove the Where { $_.EventID -eq 1212 } from the curly braces.
    This means that the command is just piping the result from the 
    remote machines into the `where` cmdlet, which filters once it 
    is done.

- If we are just processing data, normally it is better to do it 
    remotely as it reduces the computational load on the master server.

- If we are trying to pipe the data into a process, like notepad which
    needs to be opened on the local machine then this step shouldn't
    be done remotely.

### Deserialized Objects

- The downside to running commands remotely is that the objects come
    back deserialized

- This means that you cannot run the methods that normally exist once
    you have gotten the data back.

- For example, if you ran the following command

```ps
Get-Service | Get-Member
```

- And compare it against the command that we run remotely

```ps
Invoke-Command -ScriptBlock { Get-Service } -VMName Windows | Get-Member
```

- You will see that the remote execution lacks many methods.

## Remote Options

- There are also a set of options you can provide to `Invoke-Command`
    and `Enter-PSSession`.

- You can provide via the `-SessionOption` parameter

- For example, you could skip the machine name check on connection by
    providing the following SessionOptions

```ps
Enter-PSSession `
    -VMName Windows `
    -SessionOption (New-PSSessionOption -SkipCNCheck)
```