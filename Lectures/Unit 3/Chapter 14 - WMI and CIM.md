# Windows Management Instrumentation (WMI) and CIM

## WMI

- WMI is an interface for getting management information about the system.
- WMI is organized using namespaces, which are like folders for each technology type.
- For example, you may want to access DNS information which is in `root\MicrosoftDNS`
- We can see all of the namespaces by running

```ps
Get-WmiObject -Namespace Root -Class __Namespace | Select-Object -Property Name
```

- Each namespace has a set of classes that can be queried.
- We can see a list of all of the classes by running

```ps
Get-CimClass
```

### The Goods and The Bads

### Using WMI

- We can find all of the WMI commands using the following command

```ps
Get-Command -Noun WMI*
```



## CIM

- We can find all the CIM commands using

```ps
Get-Command -Module CimCmdlets
```

### WMI VS CIM

### Using CIM
