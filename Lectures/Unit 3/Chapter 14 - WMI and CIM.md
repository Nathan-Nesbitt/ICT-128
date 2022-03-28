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
Get-CimClass / Get-WmiObject
```

### Using WMI

- We can find all of the WMI commands using the following command

```ps
Get-Command -Noun WMI*
```

- If we want to search for specific WMI objects we can use the `Getr-WMIObject` cmdlet
- For example if we want a list of all objects from within the CIMv2 workspace we can run

```ps
Get-WmiObject -Namespace root\SecurityCenter2 -list
```

- Remember our use of `WHERE`, we can filter down this data

```ps
Get-WmiObject -Namespace root\SecurityCenter2 -list | where name -like '*AntiVirus*'
```

- We can also get all of the properties about this class using `Get-WMIObject`

```ps
Get-WmiObject -Namespace root/SecurityCenter2 -ClassName AntiVirusProduct
```


## CIM

### WMI VS CIM

- WMI is legacy, CIM is the newer version.
- There isn't any more development in WMI, so if you can use CIM it is best to use it

### Using CIM

- We can find all the CIM commands using

```ps
Get-Command -Module CimCmdlets
```

- We can get a list of all of all namespaces by running

```ps
Get-CimInstance -Namespace root -ClassName __Namespace
```

- We can get a list of classes using the `Get-CIMClass` cmdlet

```ps
Get-CimClass -Namespace root/SecurityCenter2
```

- If we wanted to get all of the features from one of the classes we could do so with `Get-CIMInstance`

```ps
Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntiVirusProduct | Select *
```

- We can also run scripts remotely, using our previously created VM `Windows` 

```ps
Invoke-Command -ScriptBlock { Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntiVirusProduct | Select * } -VMName Windows -Credential administrator
```
