# windowsPE Component

## Microsoft-Windows-International-Core-WinPE

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe

The Microsoft-Windows-International-Core-WinPE component is the section that specifies the default language during installation, the keyboard locale, and other international settings to use during Windows Setup or Windows Deployment Services installations.

The Microsoft-Windows-International-Core sets the same settings for the system and the user.

```
<component name="Microsoft-Windows-International-Core-WinPE">
	<SetupUILanguage>
		<UILanguage>en-GB</UILanguage> /// English International
	</SetupUILanguage>
	<InputLocale>0809:00000809</InputLocale> /// https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/default-input-locales-for-windows-language-packs
	<SystemLocale>en-GB</SystemLocale>
	<UILanguage>en-GB</UILanguage>
	<UserLocale>en-GB</UserLocale>
</component>
```

