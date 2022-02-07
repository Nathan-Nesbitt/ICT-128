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
- 
## Tables

- 

## Lists

### Regular Lists

### Wide Lists

## Custom columns and list entries

# Outputting to devices

## File

## Printer

## Host

# GridViews