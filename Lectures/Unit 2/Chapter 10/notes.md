# Formatting

## Default formatting

- If you look at the `Get-Process` command you can see that there is some fancy default
    formatting for the output.
    - The column headers have different names from the data
    - The columns have specific widths
    - They all have special formatting
- You might ask how is this done? We can do this with `.ps1xml` files!

- To see one of these files we can do the following

```ps
cd $pshome # Changes into the home directory for the powershell installation
dotnettypes.format.ps1 # Opens the default formatting file for powershell
```

- We can then search this file for the typename for the cmdlet we are running, for example
    if we want the `Get-Process` wer can run `Get-Process | gm` which returns `System.Diagnostics.Process`

- Search the file until you find an exact match, not the `ProcessModule`
- This should look something like this

```xml
<View>
    <Name>process</Name>
    <ViewSelectedBy>
        <TypeName>System.Diagnostics.Process</TypeName>
    </ViewSelectedBy>
    <TableControl>
        <TableHeaders>
            <TableColumnHeader>
                <Label>Handles</Label>
                <Width>7</Width>
                <Alignment>right</Alignment>
            </TableColumnHeader>
            <TableColumnHeader>
                <Label>NPM(K)</Label>
                <Width>7</Width>
                <Alignment>right</Alignment>
            </TableColumnHeader>
            <TableColumnHeader>
                <Label>PM(K)</Label>
                <Width>8</Width>
                <Alignment>right</Alignment>
            </TableColumnHeader>
            <TableColumnHeader>
                <Label>WS(K)</Label>
                <Width>10</Width>
                <Alignment>right</Alignment>
            </TableColumnHeader>
            <TableColumnHeader>
                <Label>VM(M)</Label>
                <Width>5</Width>
                <Alignment>right</Alignment>
            </TableColumnHeader>
            <TableColumnHeader>
                <Label>CPU(s)</Label>
                <Width>8</Width>
                <Alignment>right</Alignment>
            </TableColumnHeader>
            <TableColumnHeader>
                <Width>6</Width>
                <Alignment>right</Alignment>
            </TableColumnHeader>
            <TableColumnHeader>
                <Width>3</Width>
                <Alignment>right</Alignment>
            </TableColumnHeader>
            <TableColumnHeader />
        </TableHeaders>
        <TableRowEntries>
            <TableRowEntry>
                <TableColumnItems>
                    <TableColumnItem>
                        <PropertyName>HandleCount</PropertyName>
                    </TableColumnItem>
                    <TableColumnItem>
                        <ScriptBlock>[long]($_.NPM / 1024)</ScriptBlock>
                    </TableColumnItem>
                    <TableColumnItem>
                        <ScriptBlock>[long]($_.PM / 1024)</ScriptBlock>
                    </TableColumnItem>
                    <TableColumnItem>
                        <ScriptBlock>[long]($_.WS / 1024)</ScriptBlock>
                    </TableColumnItem>
                    <TableColumnItem>
                        <ScriptBlock>[long]($_.VM / 1048576)</ScriptBlock>
                    </TableColumnItem>
                    <TableColumnItem>
                        <ScriptBlock>if ($_.CPU -ne $()) { $_.CPU.ToString("N")}</ScriptBlock>
                    </TableColumnItem>
                    <TableColumnItem>
                        <PropertyName>Id</PropertyName>
                    </TableColumnItem>
                    <TableColumnItem>
                        <PropertyName>SI</PropertyName>
                    </TableColumnItem>
                    <TableColumnItem>
                        <PropertyName>ProcessName</PropertyName>
                    </TableColumnItem>
                </TableColumnItems>
            </TableRowEntry>
        </TableRowEntries>
    </TableControl>
</View>
```

- The process for how this formatting is applied is

    1. The cmdlet puts the object in the pipeline (`Process`)
    2. At the end of the pipeline the `Out-Default` is called
    3. `Out-Default` passes the object to `Out-Host`
    4. `Out-Host` passes the object to the formatting system
    5. The formatting system looks at the object and gets the defined rules for 
        the object which are passed back to the `Out-Host`.
    6. `Out-Host` prints out the object based on the intructions. 

- If there is no rule for the object, the system will look for default value
    for example the output from `Get-WmiObject Win32_OperatingSystem` which
    can be found inside of `Types.ps1xml`

- Finally if the system displays 4 or fewer properties, it will create a table.
    If there are more than 4, it will use a list.

## Tables

- `Format-Table` for formatting tables
- It takes in the following parameters
    - `autoSize` - Auto scales to your window `Get-WmiObject Win32_BIOS | Format-Table -autoSize`
    - `property` - Accepts a list of properties that should be included in the output
        `Get-Process | Format-Table -property ID,Name,Responding`
    - `groupBy` - Generates a new set of headers based on the property specified
        `Get-Service | Sort-Object Status | Format-Table -groupBy Status`
    - `wrap` - Wraps the contents if it overflows the screen
        `Get-Service | Format-Table Name,Status,DisplayName -autoSize -wrap`

## Lists

### Regular Lists

- `Format-List` or `Fl` is our way to format lists
- We can view the difference when running `Get-Process | Fl *` which outputs a 
    full output for the processes on the system.

### Wide Lists

- `Format-Wide` or `FW` can be used for wide lists
- Wide lists only take one column but allow you to output the result over multiple
    columns.
- For example `Get-Process | Format-Wide Id -col 3`

## Custom columns and list entries

- If we want to output custom objects we can use hash tables
- Hash tables can be created like the following

```ps
@{
    $key=$value
}
```

- We can apply this to the formatting of tables in the following format

```ps
Get-Process | Format-Table Name, 
@{
    name='VM - Megabytes'
    expression={$_.VM} 
    formatstring='F2' 
    align='right'
} 
-autosize
```

- This gets the processes and only gets the names and a new column called 
    `VM - Megabytes` which is the VM but displayed as a floating point with 2
    decimal places (F2) and instructs it to display on the right.

- If you wanna write this on one line you can separate values in the hashtable
    with `;`

# Outputting to devices

- We can see that we can output a list of processes formatted to the host using

```ps
Get-Process | Format-Wide | Out-Host
```

- We could also output to a printer or file using

```ps
Get-Process | Format-Wide | Out-File # To file
Get-Process | Format-Wide | Out-Printer # To printer
```

# GridViews

- Gridviews are not quite the same, although they allow you to format they 
    bypass the entire formatting subsystem and therefore no output goes to the screen.
- `Out-Gridview` cannot accept output from `Format-Cmdlet`