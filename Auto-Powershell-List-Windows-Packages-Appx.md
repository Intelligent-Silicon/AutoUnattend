#




```
$AppArrayList = Get-AppxPackage -PackageTypeFilter Bundle | Select-Object -Property Name, PackageFullName | Sort-Object -Property Name
 
foreach ($App in $AppArrayList) {
    # Exclude essential Windows apps  - "Microsoft.Messaging" 
    if (($App.Name -in "Microsoft.YourPhone", "Microsoft.WindowsMaps","Microsoft.Windows.Photos", "Microsoft.WindowsCalculator", "Microsoft.MicrosoftStickyNotes","Microsoft.WindowsStore", "Microsoft.Appconnector", "Microsoft.WindowsSoundRecorder", "Microsoft.DesktopAppInstaller", "Microsoft.StorePurchaseApp", "Microsoft.WindowsCamera","Microsoft.3DBuilder","Microsoft.BingWeather","Microsoft.3DViewer","Microsoft.MSPaint","Microsoft.People","Microsoft.Alarms","Microsoft.ZuneMusic","Microsoft.ZuneVideo","Microsoft.WindowsAlarms","Microsoft.ScreenSketch","Microsoft.Microsoft3DViewer","Microsoft.Print3D","Microsoft.WebMediaExtensions","Microsoft.GetHelp")) {
        Write-Output -InputObject "Skipping essential Windows app: $($App.Name)"
    }
 
    # Remove AppxPackage and AppxProvisioningPackage
    else {
        # Gather package names
        $AppPackageFullName = Get-AppxPackage -Name $App.Name | Select-Object -ExpandProperty PackageFullName
        $AppProvisioningPackageName = Get-AppxProvisionedPackage -Online | Where-Object { $_.DisplayName -like $App.Name } | Select-Object -ExpandProperty PackageName
 
        # Attempt to remove AppxPackage
        try {
            Write-Output -InputObject "Removing AppxPackage: $($AppPackageFullName)"
            Remove-AppxPackage -Package $AppPackageFullName -ErrorAction Stop
        }
        catch [System.Exception] {
            Write-Warning -Message $_.Exception.Message
        }
 
        # Attempt to remove AppxProvisioningPackage
        try {
            Write-Output -InputObject "Removing AppxProvisioningPackage: $($AppProvisioningPackageName)"
            Remove-AppxProvisionedPackage -PackageName $AppProvisioningPackageName -Online -ErrorAction Stop
        }
        catch [System.Exception] {
            Write-Warning -Message $_.Exception.Message
        }
    }
}
```



```
$applist=@(
#"Microsoft.BingWeather"
#"Microsoft.DesktopAppInstaller"
"Microsoft.GetHelp"
"Microsoft.Getstarted"
#"Microsoft.HEIFImageExtension"
"Microsoft.Messaging"
#"Microsoft.Microsoft3DViewer"
#"Microsoft.MicrosoftOfficeHub"
#"Microsoft.MicrosoftSolitaireCollection"
#"Microsoft.MicrosoftStickyNotes"
"Microsoft.MixedReality.Portal"
#"Microsoft.MSPaint"
#"Microsoft.Office.OneNote"
"Microsoft.OneConnect"
"Microsoft.People"
#"Microsoft.Print3D"
#"Microsoft.ScreenSketch"
"Microsoft.SkypeApp"
"Microsoft.StorePurchaseApp"
#"Microsoft.VP9VideoExtensions"
"Microsoft.Wallet"
#"Microsoft.WebMediaExtensions"
#"Microsoft.WebpImageExtension"
#"Microsoft.Windows.Photos"
#"Microsoft.WindowsAlarms"
#"Microsoft.WindowsCalculator"
#"Microsoft.WindowsCamera"
"microsoft.windowscommunicationsapps"
#"Microsoft.WindowsFeedbackHub"
#"Microsoft.WindowsMaps"
#"Microsoft.WindowsSoundRecorder"
#"Microsoft.WindowsStore"
"Microsoft.Xbox.TCUI"
"Microsoft.XboxApp"
"Microsoft.XboxGameOverlay"
"Microsoft.XboxGamingOverlay"
"Microsoft.XboxIdentityProvider"
"Microsoft.XboxSpeechToTextOverlay"
"Microsoft.YourPhone"
"Microsoft.ZuneMusic"
"Microsoft.ZuneVideo"
)
foreach ($appx in $applist) {Get-AppXProvisionedPackage -online | where DisplayName -EQ $appx | Remove-AppxProvisionedPackage -online}
```