# Remote Connect via Terminal

## Example Clients

### Windows

* Windows Subsystem for Linux (WSL)
  * Need to install a Linux environment through Microsoft Store
  * Creates a Linux native terminal environment

* SSH through PowerShell
  * Requires PowerShell 6 or higher and OpenSSH client must be installed
  * PowerShell must be ran as administrator to use OpenSSH client

### MacOSX

* Has a native terminal client called ... "Terminal"
  * Uses zsh natively
* Another popular alternative terminal program is iTerm2

### Linux

* Comes with a native terminal
  * Depending on Linux distro (i.e. Ubuntu, Fedora, OpenSuse, ArchLinux, etc.) and the desktop environment, the name of the terminal program varies
  * Also depending on the distro, there are additional terminal client programs available (i.e. terminator, guake, etc.)


## Example Commands

### Standard SSH connection

```
ssh root@<ip_address>
```
