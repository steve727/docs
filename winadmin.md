### Windows Admin Workstation Tools & other usual apps for d2d ops
```
Import-Module ServerManager
Install-WindowsFeature -Name RSAT-AD-Powershell
```

WSL2
```
wsl --install
shutdown /r /t 0
wsl --list --online
wsl --install -d Debian
wsl --update
wsl --shutdown
```

[Microsoft Terminal](https://github.com/microsoft/terminal)

```
wsl --set-default-version 2
https://aka.ms/wsl-debian-gnulinux
Add-AppxPackage .\TheDebianProject.DebianGNULinux_1.3.0.0.AppxBundle
Add-AppxPackage .\Microsoft.WindowsTerminal_1.7.1091.0_8wekyb3d8bbwe.msixbundle
```

[AdoptOpenJDK](https://adoptopenjdk.net/?variant=openjdk15&jvmVariant=hotspot)

[VMware plug-in](http://vsphereclient.vmware.com/vsphereclient/VMware-EnhancedAuthenticationPlugin-6.7.0.exe)

[Putty](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.74-installer.msi)

[Remote Desktop Manager Free](https://cdn.devolutions.net/download/Setup.RemoteDesktopManagerFree.2020.3.23.0.msi)

[Winscp](https://winscp.net/download/WinSCP-5.17.9-Setup.exe)

[1password](https://app-updates.agilebits.com/download/OPW7)

[VSCode](https://code.visualstudio.com/docs/?dv=win64)
```
enable-crash-reporter = false
telemetry.enableTelemetry = false
```
[Postman](https://dl.pstmn.io/download/latest/win64)

[Terraform](https://releases.hashicorp.com/terraform/0.15.0/terraform_0.15.0_windows_amd64.zip)

[Git](https://github.com/git-for-windows/git/releases/download/v2.29.2.windows.3/Git-2.29.2.3-64-bit.exe)

[awscli2](https://awscli.amazonaws.com/AWSCLIV2.msi)

[az cli](https://aka.ms/installazurecliwindows)

[Wireshark](https://1.na.dl.wireshark.org/win64/Wireshark-win64-3.4.2.exe)

[AngryIP Scanner](https://github.com/angryip/ipscan/releases/download/3.7.3/ipscan-3.7.3-setup.exe)

[Azure Storage Explorer](https://go.microsoft.com/fwlink/?LinkId=708343&clcid=0x409)

[Rufus](https://github.com/pbatard/rufus/releases/download/v3.13/rufus-3.13.exe)

[RVTools 4.1.3](https://www.robware.net/rvtools/download)

### TODO:
```
Chrome
DCNM Client
Fiddler
Office
Visual Studio
```
### Server builds
```
@('EnableLUA','ConsentPromptBehaviorAdmin','FilterAdministratorToken') | ForEach-Object{ ` Set-ItemProperty -Path /
REGISTRY::HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System -Name $_ -Value 0 ;} /
;Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False ;winrm qc -q ;winrm s winrm/config/client /
'@{TrustedHosts="*"}' ;Enable-PSRemoting -Confirm:$false'
```
### Powershell
`Get-AdUser -Filter * -Properties DisplayName | Select-Object DisplayName | Export-Csv 'c:\ad.csv'`

### Install updates
`Install-WindowsUpdate -MicrosoftUpdate -AcceptAll -AutoReboot`
