# Get Installed Programs With PowerShell

While updating a server with an old operating system ( Windows Server 2008 R2 Standard ), we decided to replace it with another one with Windows 2019 Standart Edition.
Naturally, all the programs on the old computer had to be installed on the new one. Meanwhile, I needed a list of the programs on the server.

The following script lists the installed programs on the computer and saves it to a `CSV` file. The file named with computer name will be created with in the folder where you run the script. You can save it as a `.ps1` file and run.

```
Clear-Host;
$computerName = Get-WMIObject Win32_ComputerSystem | Select-Object -ExpandProperty name;
$outputFile = "$computerName.csv";
if (Test-Path $outputFile) { Remove-Item $outputFile; }
Add-Content $outputFile "DisplayName;DisplayVersion;Publisher;InstallDate";
$applications = Get-ItemProperty "HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*";
foreach ($application in $applications)
{
    $displayName = $application.DisplayName;
    $displayVersion = $application.DisplayVersion;
    $publisher = $application.Publisher;
    $installDate = $application.InstallDate;
    if ($displayName -ne $null -or $displayVersion -ne $null -or $publisher -ne $null -or $installDate -ne $null)
    {
        Add-Content $outputFile "$displayName;$displayVersion;$publisher;$installDate";
    }
}
```
