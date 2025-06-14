```
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\Users\Admin1\Documents\WIM files\installW11.wim" /mountdir:"C:\mount" /index:7

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Mounting image
[==========================100.0%==========================]
The operation completed successfully.
PS C:\WINDOWS\system32> Dism /Image:"C:\mount" /Get-Drivers

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Image Version: 10.0.26100.2033

Obtaining list of 3rd party drivers from the driver store...

Driver packages listing:

(No drivers found in the image matching the criteria)

The operation completed successfully.
PS C:\WINDOWS\system32> Dism /Image:"C:\mount" /Add-Driver /Driver:"C:\drivers\Netwtw08.INF"

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Image Version: 10.0.26100.2033

Found 1 driver package(s) to install.
Installing 1 of 1 - C:\drivers\Netwtw08.INF: The driver package was successfully installed.
The operation completed successfully.
PS C:\WINDOWS\system32> Dism /Image:"C:\mount" /Get-Drivers

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Image Version: 10.0.26100.2033

Obtaining list of 3rd party drivers from the driver store...

Driver packages listing:

Published Name : oem0.inf
Original File Name : netwtw08.inf
Inbox : No
Class Name : net
Provider Name : Intel
Date : 17/05/2024
Version : 23.60.0.10

The operation completed successfully.
PS C:\WINDOWS\system32> Dism /Image:"C:\mount" /Add-Driver /Driver:"C:\AllSignedDrivers" /Recurse

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Image Version: 10.0.26100.2033

Searching for driver packages to install...
Found 40 driver package(s) to install.
Installing 1 of 40 - C:\AllSignedDrivers\audio\20252505.14005618\Source\ExtRtk_9553.1\HDX_LenovoExt_DOLBY_Wrap_ELEVOC_Whisper3.0.inf: The driver package was successfully installed.
Installing 2 of 40 - C:\AllSignedDrivers\audio\20252505.14005618\Source\Realtek\Codec_9553.1\HDXLV.inf: The driver package was successfully installed.
Installing 3 of 40 - C:\AllSignedDrivers\audio\20252505.14005618\Source\Realtek\RealtekAPO_13_1097\RealtekAPO.inf: The driver package was successfully installed.
Installing 4 of 40 - C:\AllSignedDrivers\audio\20252505.14005618\Source\Realtek\RealtekService_631\RealtekService.inf: The driver package was successfully installed.
Installing 5 of 40 - C:\AllSignedDrivers\audio\20252505.14005618\Source\Thirdparty\D1125\ext_lenovo_AIO_rtk_20h1_21h2_22h2_v7.1123.1527.44\dax3_ext_rtk.inf: The driver package was successfully installed.
Installing 6 of 40 - C:\AllSignedDrivers\audio\20252505.14005618\Source\Thirdparty\D1125\swc_factory\swc_aposvc_20h1_21h2_22h2_b20402_v3.30502.532.0\dax3_swc_aposvc.inf: The driver package was successfully installed.
Installing 7 of 40 - C:\AllSignedDrivers\audio\20252505.14005618\Source\Thirdparty\D1125\swc_factory\swc_hsa_19h1_20h1_21h2_22h2_b14302_v3.30201.210.0\dax3_swc_hsa.inf: The driver package was successfully installed.
Installing 8 of 40 - C:\AllSignedDrivers\audio\20252505.14005618\Source\Thirdparty\E0828\ElevocAPO64\ElevocAPO64.inf: The driver package was successfully installed.
Installing 9 of 40 - C:\AllSignedDrivers\audio\20252505.14005618\Source\Thirdparty\E0828\ElevocAPO64Ext\ElevocAPO64Ext.inf: The driver package was successfully installed.
Installing 10 of 40 - C:\AllSignedDrivers\bluetooth\20252505.14020461\Source\Intel_BT_23.60.0.1_WHQL\CCP\ibtusb.inf: The driver package was successfully installed.
Installing 11 of 40 - C:\AllSignedDrivers\bluetooth\20252505.14020461\Source\Intel_BT_23.60.0.1_WHQL\GAP\ibtusb.inf: The driver package was successfully installed.
Installing 12 of 40 - C:\AllSignedDrivers\bluetooth\20252505.14020461\Source\Intel_BT_23.60.0.1_WHQL\GFP\ibtusb.inf: The driver package was successfully installed.
Installing 13 of 40 - C:\AllSignedDrivers\bluetooth\20252505.14020461\Source\Intel_BT_23.60.0.1_WHQL\HRP\ibtusb.inf: The driver package was successfully installed.
Installing 14 of 40 - C:\AllSignedDrivers\bluetooth\20252505.14020461\Source\Intel_BT_23.60.0.1_WHQL\JFP\ibtusb.inf: The driver package was successfully installed.
Installing 15 of 40 - C:\AllSignedDrivers\bluetooth\20252505.14020461\Source\Intel_BT_23.60.0.1_WHQL\MTP\ibtusb.inf: The driver package was successfully installed.
Installing 16 of 40 - C:\AllSignedDrivers\bluetooth\20252505.14020461\Source\Intel_BT_23.60.0.1_WHQL\THP\ibtusb.inf: The driver package was successfully installed.
Installing 17 of 40 - C:\AllSignedDrivers\bluetooth\20252505.14020461\Source\Intel_BT_23.60.0.1_WHQL\TYP\ibtusb.inf: The driver package was successfully installed.
Installing 18 of 40 - C:\AllSignedDrivers\bluetooth\20252505.14020461\Source\Realtek_BT_8822CE_1.9.1051.3006_WHQL\Rtkfilter.inf: The driver package was successfully installed.
Installing 19 of 40 - C:\AllSignedDrivers\bluetooth\20252505.14020461\Source\Realtek_BT_8852BE_2.10.1061.3015_WHQL\Rtkfilter.inf: The driver package was successfully installed.
Installing 20 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Realtek\RtLeJA.inf: The driver package was successfully installed.
Installing 21 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Realtek\RtLeJE.inf: The driver package was successfully installed.
Installing 22 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Realtek\RtLeJF.inf: The driver package was successfully installed.
Installing 23 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Realtek\RtLeJFIR.inf: The driver package was successfully installed.
Installing 24 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Realtek\RtLeShA.inf: The driver package was successfully installed.
Installing 25 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Realtek\RtLeShF.inf: The driver package was successfully installed.
Installing 26 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Realtek\RtLeShL.inf: The driver package was successfully installed.
Installing 27 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Realtek\RtLeShS.inf: The driver package was successfully installed.
Installing 28 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Realtek\RtLeShV.inf: The driver package was successfully installed.
Installing 29 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Realtek\RtLeSLF.inf: The driver package was successfully installed.
Installing 30 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Sonix\snDMFT.inf: The driver package was successfully installed.
Installing 31 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Sunplus\SPUVCbvCN.inf: The driver package was successfully installed.
Installing 32 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Sunplus\SPUVCbvCN1.inf: The driver package was successfully installed.
Installing 33 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Sunplus\SPUVCbvCN2.inf: The driver package was successfully installed.
Installing 34 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Sunplus\SPUVCbvCN3.inf: The driver package was successfully installed.
Installing 35 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Sunplus\SPUVCbvJP.inf: The driver package was successfully installed.
Installing 36 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Sunplus\SPUVCbvJPIR.inf: The driver package was successfully installed.
Installing 37 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Sunplus\SPUVCbvMerge.inf: The driver package was successfully installed.
Installing 38 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Sunplus\SPUVCbvMerge1.inf: The driver package was successfully installed.
Installing 39 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Sunplus\SPUVCbvMerge2.inf: The driver package was successfully installed.
Installing 40 of 40 - C:\AllSignedDrivers\camera\20252505.14022656\Source\Sunplus\SPUVCbvMergeIR.inf: The driver package was successfully installed.
The operation completed successfully.
PS C:\WINDOWS\system32> Dism /Image:"C:\mount" /Get-Drivers

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Image Version: 10.0.26100.2033

Obtaining list of 3rd party drivers from the driver store...

Driver packages listing:

Published Name : oem0.inf
Original File Name : netwtw08.inf
Inbox : No
Class Name : net
Provider Name : Intel
Date : 17/05/2024
Version : 23.60.0.10

Published Name : oem1.inf
Original File Name : hdx_lenovoext_dolby_wrap_elevoc_whisper3.0.inf
Inbox : No
Class Name : Extension
Provider Name : Realtek Semiconductor Corp.
Date : 25/07/2023
Version : 6.0.9553.1

Published Name : oem10.inf
Original File Name : ibtusb.inf
Inbox : No
Class Name : Bluetooth
Provider Name : Intel Corporation
Date : 01/05/2024
Version : 23.60.0.1

Published Name : oem11.inf
Original File Name : ibtusb.inf
Inbox : No
Class Name : Bluetooth
Provider Name : Intel Corporation
Date : 01/05/2024
Version : 23.60.0.1

Published Name : oem12.inf
Original File Name : ibtusb.inf
Inbox : No
Class Name : Bluetooth
Provider Name : Intel Corporation
Date : 01/05/2024
Version : 23.60.0.1

Published Name : oem13.inf
Original File Name : ibtusb.inf
Inbox : No
Class Name : Bluetooth
Provider Name : Intel Corporation
Date : 01/05/2024
Version : 23.60.0.1

Published Name : oem14.inf
Original File Name : ibtusb.inf
Inbox : No
Class Name : Bluetooth
Provider Name : Intel Corporation
Date : 01/05/2024
Version : 23.60.0.1

Published Name : oem15.inf
Original File Name : ibtusb.inf
Inbox : No
Class Name : Bluetooth
Provider Name : Intel Corporation
Date : 01/05/2024
Version : 23.60.0.1

Published Name : oem16.inf
Original File Name : ibtusb.inf
Inbox : No
Class Name : Bluetooth
Provider Name : Intel Corporation
Date : 01/05/2024
Version : 23.60.0.1

Published Name : oem17.inf
Original File Name : ibtusb.inf
Inbox : No
Class Name : Bluetooth
Provider Name : Intel Corporation
Date : 01/05/2024
Version : 23.60.0.1

Published Name : oem18.inf
Original File Name : rtkfilter.inf
Inbox : No
Class Name : Bluetooth
Provider Name : Realtek Semiconductor Corp.
Date : 12/07/2023
Version : 1.9.1051.3006

Published Name : oem19.inf
Original File Name : rtkfilter.inf
Inbox : No
Class Name : Bluetooth
Provider Name : Realtek Semiconductor Corp.
Date : 20/03/2024
Version : 2.10.1061.3015

Published Name : oem2.inf
Original File Name : hdxlv.inf
Inbox : No
Class Name : MEDIA
Provider Name : Realtek Semiconductor Corp.
Date : 25/07/2023
Version : 6.0.9553.1

Published Name : oem20.inf
Original File Name : rtleja.inf
Inbox : No
Class Name : Camera
Provider Name : Realtek
Date : 28/06/2024
Version : 10.0.22000.20332

Published Name : oem21.inf
Original File Name : rtleje.inf
Inbox : No
Class Name : Camera
Provider Name : Realtek
Date : 28/06/2024
Version : 10.0.22000.20332

Published Name : oem22.inf
Original File Name : rtlejf.inf
Inbox : No
Class Name : Camera
Provider Name : Realtek
Date : 28/06/2024
Version : 10.0.22000.20332

Published Name : oem23.inf
Original File Name : rtlejfir.inf
Inbox : No
Class Name : Camera
Provider Name : Realtek
Date : 28/06/2024
Version : 10.0.22000.20332

Published Name : oem24.inf
Original File Name : rtlesha.inf
Inbox : No
Class Name : Camera
Provider Name : Realtek
Date : 28/06/2024
Version : 10.0.22000.20332

Published Name : oem25.inf
Original File Name : rtleshf.inf
Inbox : No
Class Name : Camera
Provider Name : Realtek
Date : 28/06/2024
Version : 10.0.22000.20332

Published Name : oem26.inf
Original File Name : rtleshl.inf
Inbox : No
Class Name : Camera
Provider Name : Realtek
Date : 28/06/2024
Version : 10.0.22000.20332

Published Name : oem27.inf
Original File Name : rtleshs.inf
Inbox : No
Class Name : Camera
Provider Name : Realtek
Date : 28/06/2024
Version : 10.0.22000.20332

Published Name : oem28.inf
Original File Name : rtleshv.inf
Inbox : No
Class Name : Camera
Provider Name : Realtek
Date : 28/06/2024
Version : 10.0.22000.20332

Published Name : oem29.inf
Original File Name : rtleslf.inf
Inbox : No
Class Name : Camera
Provider Name : Realtek
Date : 28/06/2024
Version : 10.0.22000.20332

Published Name : oem3.inf
Original File Name : realtekapo.inf
Inbox : No
Class Name : AudioProcessingObject
Provider Name : Realtek
Date : 19/07/2023
Version : 13.0.6000.1097

Published Name : oem30.inf
Original File Name : sndmft.inf
Inbox : No
Class Name : Camera
Provider Name : Sonix
Date : 07/05/2024
Version : 10.13.22621.23

Published Name : oem31.inf
Original File Name : spuvcbvcn.inf
Inbox : No
Class Name : Camera
Provider Name : SunplusIT
Date : 16/07/2024
Version : 5.0.18.237

Published Name : oem32.inf
Original File Name : spuvcbvcn1.inf
Inbox : No
Class Name : Camera
Provider Name : SunplusIT
Date : 16/07/2024
Version : 5.0.18.237

Published Name : oem33.inf
Original File Name : spuvcbvcn2.inf
Inbox : No
Class Name : Camera
Provider Name : SunplusIT
Date : 16/07/2024
Version : 5.0.18.237

Published Name : oem34.inf
Original File Name : spuvcbvcn3.inf
Inbox : No
Class Name : Camera
Provider Name : SunplusIT
Date : 16/07/2024
Version : 5.0.18.237

Published Name : oem35.inf
Original File Name : spuvcbvjp.inf
Inbox : No
Class Name : Camera
Provider Name : SunplusIT
Date : 16/07/2024
Version : 5.0.18.237

Published Name : oem36.inf
Original File Name : spuvcbvjpir.inf
Inbox : No
Class Name : Camera
Provider Name : SunplusIT
Date : 16/07/2024
Version : 5.0.18.237

Published Name : oem37.inf
Original File Name : spuvcbvmerge.inf
Inbox : No
Class Name : Camera
Provider Name : SunplusIT
Date : 16/07/2024
Version : 5.0.18.237

Published Name : oem38.inf
Original File Name : spuvcbvmerge1.inf
Inbox : No
Class Name : Camera
Provider Name : SunplusIT
Date : 16/07/2024
Version : 5.0.18.237

Published Name : oem39.inf
Original File Name : spuvcbvmerge2.inf
Inbox : No
Class Name : Camera
Provider Name : SunplusIT
Date : 16/07/2024
Version : 5.0.18.237

Published Name : oem4.inf
Original File Name : realtekservice.inf
Inbox : No
Class Name : SoftwareComponent
Provider Name : Realtek
Date : 24/07/2023
Version : 1.0.631.0

Published Name : oem40.inf
Original File Name : spuvcbvmergeir.inf
Inbox : No
Class Name : Camera
Provider Name : SunplusIT
Date : 16/07/2024
Version : 5.0.18.237

Published Name : oem5.inf
Original File Name : dax3_ext_rtk.inf
Inbox : No
Class Name : Extension
Provider Name : Dolby
Date : 22/11/2022
Version : 7.1123.1527.44

Published Name : oem6.inf
Original File Name : dax3_swc_aposvc.inf
Inbox : No
Class Name : AudioProcessingObject
Provider Name : Dolby
Date : 16/11/2022
Version : 3.30502.532.0

Published Name : oem7.inf
Original File Name : dax3_swc_hsa.inf
Inbox : No
Class Name : SoftwareComponent
Provider Name : Dolby
Date : 14/10/2021
Version : 3.30201.210.0

Published Name : oem8.inf
Original File Name : elevocapo64.inf
Inbox : No
Class Name : AudioProcessingObject
Provider Name : Elevoc Technology Co.,Ltd
Date : 25/08/2023
Version : 3.0.3.154

Published Name : oem9.inf
Original File Name : elevocapo64ext.inf
Inbox : No
Class Name : Extension
Provider Name : Elevoc Technology Co.,Ltd
Date : 11/07/2023
Version : 1.0.2.157

The operation completed successfully.
PS C:\WINDOWS\system32>
```