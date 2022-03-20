# Security

## What Powershell Does/Doesn't Do

- Powershell doesn't add any additional layers of security, it only 
  allows you to work on and run stuff you already have permissions to
  access.
- Powershell will not stop users from typing anything in.
- Powershell does try to stop users from **unintensionally** running commands.
- Powershell does not protect against malware.

## Execution Policy

- The default setting for execution policy is **Restricted** which means you can't run **scripts**
- You can still run command when in restricted mode

### View Execution Policy

```ps
Get-ExecutionPolicy
```

### Setting Execution Policy

```ps
Set-ExecutionPolicy <POLICY>
```

- The policy can be one of the following:
  - `Restricted` - Default, no scipts can be run (other than some microsoft supplied scripts).
  - `AllSigned` - Will run any script that has been signed by any trusted certificate authority (CA).
  - `RemoteSigned` - Will execute any local script, and will run any remote script if
  - `Unrestricted` - All scripts will run.
  - `Bypass` - Ignores all security (intended for development, don't use this)

- The recommended setting is `RemoteSigned` when you want to run scripts. Everything else should be left at Restricted.

## Digital Code Signing

- You can 

## Auto-Run

## Script Execution

## Other Security Holes
