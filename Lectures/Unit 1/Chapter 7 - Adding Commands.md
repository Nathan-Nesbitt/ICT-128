# Adding Commands

## Management Shells

- When you install a program it will often provide you with CLI tools
- These tools look like separate programs but they are realy just snap-ins or modules
    that are run when you start.
- They are not separate from the regular powershell instance.
- There are exceptions where products bundle a specific shell for use, for example
    `SQL Server`.

## Finding and Adding 
### Snap-Ins

- Technically the name for a snap-in is `PSSnapin`
- We can get a list of avalible snap-ins by running `Get-PSSnapin -registered`
- We can load a snap-in by running `Add-PSSnapin sqlservercmdletsnapin100`
- We can then view the cmdlets we can run `Get-Command -pssnapin sqlservercmdletsnapin100` 
    or `gcm -pssnapin sqlservercmdletsnapin100`.
- If we wanna see if the snap-in provides any new PSDrive providers we can run 
    `Get-PSProvider` 
- We can remove a snap-in by running `Remove-PSSnapin`

### Modules

- Modules are the newer way of importing external packages
- This is how most programming languages handle external commands / functions
- We have a central location where powershell looks for modules `Get-Content Env:/PSModulePath`
- This is how Get-Help discovers the commands and help files on your system.
- If you don't have a module loaded you can run `Import-Module <PATH-TO-MODULE>`
- You can even load in your own custom modules!
- We can remove a module by running `Remove-Module`

## Pre-loading modules

### Console File

- If you don't want to manually get and load all of your modules/snap-ins you can create a console file
- To create a console file you can do the following:
    1. Import all of the snap-ins and modules you want.
    2. Run this command to export the shell `Export-Console c:\myshell.psc`
    3. Create a new powershell shortcut which points to `%windir%\system32\WindowsPowerShell\v1.0\powershell.exe -noexit -psconsolefile c:\myshell.psc`
- This creates a copy of the current shell, exports the modules you installed, then uses the XML 
    file that was created to re-generate your shell every time you click the shortcut. 

### Profile Scripts

- We can also create a profile script to accomplish a pre-load
- Before you start this section, ensure you can run scripts by running `Set-ExecutionPolicy RemoteSigned`
    as admin

    1. Create the folder `WindowsPowerShell` folder in your `Documents` directory
    2. Create a file called `profile.ps1` in the `WindowsPowerShell` directory
    3. Open the `profile.ps1` file and add in all of your `Add-PSSnapin` and `Import-Module` code 
    4. Close the shell you are using, and re-open it.

## Getting modules from the internet

- You can also download scripts from the internet using the `PowerShellGet`
- You can see a list of possible scripts [here](http://powershellgallery.com)
- These scripts are not verified by microsoft.

    1. You can add a URL of a repository by running `Register-PSRepository`
    2. You can search for modules by running `Find-Module`
    3. You can install a module by running `Install-Module`
    4. You can update a module by running `Update-Module`

