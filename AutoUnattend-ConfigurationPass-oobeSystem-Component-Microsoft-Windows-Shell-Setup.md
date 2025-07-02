```
<component name="Microsoft-Windows-Shell-Setup">
	<HideEULAPage>true</HideEULAPage>								/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe-hideeulapage
	<HideOEMRegistrationScreen>true</HideOEMRegistrationScreen>		/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe-hideoemregistrationscreen
	<HideOnlineAccountScreens>true</HideOnlineAccountScreens>		/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe-hideonlineaccountscreens
	<HideWirelessSetupInOOBE>true</HideWirelessSetupInOOBE>			/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe-hidewirelesssetupinoobe
	<ProtectYourPC>3</ProtectYourPC>								/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe-protectyourpc

	<Themes>												/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-themes
															/// Changing the Default Window Theme needs the DesktopBackground and ThemeName elements.
		<DesktopBackground></DesktopBackground>				/// Can be left blank https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-themes-desktopbackground
		<ThemeName>dark.theme</ThemeName>					/// Cant be left blank . Location:C:\Windows\Resources\Themes https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-themes-themename
		<UWPAppsUseLightTheme>false</UWPAppsUseLightTheme> 	/// Dark Mode https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-themes-uwpappsuselighttheme
	</Themes>

	<UserAccounts>									/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts
		<LocalAccounts>								/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts-localaccounts
			<LocalAccount wcm:action="add">			/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts-localaccounts-localaccount
				<Name>Admin1</Name>					/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts-localaccounts-localaccount-name
				<DisplayName></DisplayName>			/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts-localaccounts-localaccount-displayname
				<Group>Administrators</Group>		/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts-localaccounts-localaccount-group
				<Password>							/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts-localaccounts-localaccount-password
					<Value>admin1</Value>			/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts-localaccounts-localaccount-password-value
					<PlainText>true</PlainText>		/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts-localaccounts-localaccount-password-plaintext
				</Password>
			</LocalAccount>
			<LocalAccount wcm:action="add">
				<Name>User1</Name>
				<DisplayName></DisplayName>
				<Group>Users</Group>
				<Password>
					<Value>user1</Value>
					<PlainText>true</PlainText>
				</Password>
			</LocalAccount>
		</LocalAccounts>
	</UserAccounts>
</component>
```