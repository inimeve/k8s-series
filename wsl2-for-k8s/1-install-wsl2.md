# Installation of WSL2

## Enable WSL feature
Run in powershell as admin:
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

## Enable Virtual Machine feature
Run in powershell as admin:
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

## Download linux kernel update package
Download the lastest pacakge:
[WSL2 kernel update](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

## Set WSL default version to 2
Run in powershell as admin:
```
wsl --set-default-version 2
```

## Install linux distro
Install the distro of choice:
[Microsoft Store](https://aka.ms/wslstore)