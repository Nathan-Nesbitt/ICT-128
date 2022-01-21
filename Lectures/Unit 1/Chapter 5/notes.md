# Providers

## What are providers?

- Providers aka `PSProviders` is an adapter to make things look like drives
- You can get a list of providers by running `Get-PSProvider`
- The providers have a list of possible capabilities
    - `ShouldProcess` - Test actions before you perform them
    - `Filter` - Allows you to 
    - `Credentials` - Allows you to provide credentials for the datastore

- We can also work with `PSDrives` which uses a single provider to connect to drives connected to the system.

## Understanding the filesystem organization

- Main components of the filesystem
    - Drives
    - Folders
    - Files

- The terminology is different for PowerShell as we might not always be working with a filesystem.
- We refer to Folders and Files as `Item`s 
- We can access attributes of `Items` using the `ItemProperty` noun (e.g. `Get-ItemProperty`)
- We can get the children of an `Item` using the `ChildItem` noun (e.g. `Get-ChildItem`)


## Navigating the filesystem

- We may want to move around the filesystem
- We can do this by running `Set-Location`
- We can also get the current location using `Get-Location`
- The alias for the change directory command is `cd`
- We can also create new files and folders by running 
    - `New-Item -Type directory my_directory` - To create a new directory called `my_directory`
    - `New-Item my_file` - To create a new file called `my_file`

## Wildcards and Literal Paths

- If we want to get a range of files matching a specific pattern we can use wildcards:
    - `*` - Any number of characters
    - `?` - A single character
- Note that if you use the `LiteralPath` parameter to any of these functions, the wildcard will not work.
    This only works with the `Path` parameter.

For example:

`dir *.md` would only return any files in the current directory that end in .md

## Other Providers

- Another provider is the registry. 
- We can access this by running `Set-Location -Path hkcu:`
- We can view the values in the registry by running `Get-ChildItem`
- We can modify values in the registry by running `Set-ItemProperty -Path dwm -PSProperty EnableAeroPeek -Value 0`