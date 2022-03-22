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

- Code signing is the process of generating a hash of the script using a signature.
- This provides two things:
  1. Identifies who created the script (or rather who signed it)
  2. Ensure the integrity of the script (makes sure it hasn't been changed)
- You can sign your own scripts using a class 3 certificate, which can be obtained from a certificate 
  authority (CA) or a internal public-key infrastructure (PKI)
- You should only trust a signature if the CA that authorized it is reputable.
- You can view all the CAs on the system via `Manage Computer Certificates`
  - The root authorities can be viewed under the `Trusted Root Certification Authorities` tab
- Any certificate provided by these authorities is trusted automatically.

### Signing a script

1. Create a new self-signed certificate using `New-SelfSignedCertificate` or get one from your CA

```ps
New-SelfSignedCertificate `
  -Subject "Cert for Code Signing” `
  -Type CodeSigningCert `
  -DnsName test1 `
  -CertStoreLocation cert:\LocalMachine\My
```

2. Sign the script using the new certificate

```ps
$cert = (Get-ChildItem cert:\LocalMachine\my -CodeSigningCert)[0]
Set-AuthenticodeSignature <SCRIPT> $cert
```

- This will throw an error as the file is self-signed. But you should be able to check the contents
  of the file for the signature. 

## Auto-Run

- Auto run is disabled by default on the system for powershell files
- This is to ensure that people are not tricked into running scripts.
- This also applies to scripts run from the CLI, you cannot change the name of a script to run
    without specifying the full path
- This means that you cannot create a new script called "ls" that runs arbitrary code
- This also means you need to run the script using a path `./YOURSCRIPT.PS1`

## Other Security Holes

- Be aware that powershell doesn't stop users from typing commands in
- This means that social engineering attacks are always a threat
- Users can be convinced via mail or phone (or any other platform) to
    breach the security of your system.
- Train people to detect and avoid falling for these tricks.
