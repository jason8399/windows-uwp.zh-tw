---
author: TylerMSFT
title: 保留檔案和 URI 配置名稱
description: 此主題列出您的應用程式無法使用的保留檔案和 URI 配置名稱。
ms.assetid: 7428C4A2-1380-4EBB-9C2A-7DF7B5C468AE
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 98735ce2d102c1a0b7b054b9d70038fe5f9dc40c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.locfileid: "210804"
---
# <a name="reserved-file-and-uri-scheme-names"></a>保留檔案和 URI 配置名稱

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

您可以使用 URI 關聯，在另一個 app 啟動特定 URI 配置時自動啟動您的 app。 但是有些 URI 關聯是您無法使用的，因為它們是保留的關聯。 如果您的 app 登錄為保留的關聯，該登錄將會被忽略。 此主題列出並不適用於您的 app 的保留檔案和 URI 配置名稱。

## <a name="reserved-file-types"></a>保留的檔案類型

保留的檔案類型有兩種類型：保留給內建 app 的檔案類型，和保留給作業系統的檔案類型。 當保留給內建 app 的檔案類型啟動時，將只會啟動內建 app。 您的 app 若嘗試對該檔案類型登錄，將一律被忽略。 同樣地，您的 app 若嘗試對保留給作業系統的檔案類型進行登錄，也將一律被忽略。

保留給內建 app 的檔案類型

<table>
<tr><td>.aac</td><td>.icon</td><td>.pem</td><td>.wdp</td></tr>
<tr><td>.aetx</td><td>.jpeg</td><td>.png</td><td>.wmv</td></tr>
<tr><td>.asf</td><td>.jxr</td><td>.pptm</td><td>.xap</td></tr>
<tr><td>.bmp</td><td>.m4a</td><td>.pptx</td><td>.xht</td></tr>
<tr><td>.cer</td><td>.m4r</td><td>.qcp</td><td>.xhtml</td></tr>
<tr><td>.dotm</td><td>.m4v</td><td>.rtf</td><td>.xltm</td></tr>
<tr><td>.dotx</td><td>.mov</td><td>.tif</td><td>.xltx</td></tr>
<tr><td>.gif</td><td>.mp3</td><td>.tiff</td><td>.xml</td></tr>
<tr><td>.hdp</td><td>.mp4</td><td>.txt</td><td>.xsl</td></tr>
<tr><td>.htm</td><td>.one</td><td>.url</td><td>.zip</td></tr>
<tr><td>.html</td><td>.onetoc2</td><td>.vcf</td><td></td></tr>
<tr><td>.ico</td><td>.p7b</td><td>.wav</td><td></td></tr>
</table> 

## <a name="file-types-reserved-for-the-operating-system"></a>保留給作業系統的檔案類型

以下是保留給作業系統的檔案類型

<table>
<tr><td>.accountpicture-ms</td><td>its</td><td>.ops</td><td>.url</td></tr>
<tr><td>.ade</td><td>.jar</td><td>.pcd</td><td>.vb</td></tr>
<tr><td>.adp</td><td>.js</td><td>.pif</td><td>.vbe</td></tr>
<tr><td>.app</td><td>.jse</td><td>.pl</td><td>.vbp</td></tr>
<tr><td>.appx</td><td>.ksh</td><td>.plg</td><td>.vbs</td></tr>
<tr><td>.application</td><td>.lnk</td><td>.plsc</td><td>.vhd</td></tr>
<tr><td>.appref-ms</td><td>.mad</td><td>.prf</td><td>.vhdx</td></tr>
<tr><td>.asp</td><td>.maf</td><td>.prg</td><td>.vsmacros</td></tr>
<tr><td>.bas</td><td>.mag</td><td>.printerexport</td><td>.vsw</td></tr>
<tr><td>.bat</td><td>.mam</td><td>.provxml</td><td>.webpnp</td></tr>
<tr><td>.cab</td><td>.maq</td><td>.ps1</td><td>.ws</td></tr>
<tr><td>.chm</td><td>.mar</td><td>.ps1xml</td><td>.wsc</td></tr>
<tr><td>.cmd</td><td>.mas</td><td>.ps2</td><td>.wsf</td></tr>
<tr><td>.cnt</td><td>.mat</td><td>.ps2xml</td><td>.wsh</td></tr>
<tr><td>.com</td><td>.mau</td><td>.psc1</td><td>.xaml</td></tr>
<tr><td>.cpf</td><td>.mav</td><td>.psc2</td><td>.xdp</td></tr>
<tr><td>.cpl</td><td>.maw</td><td>.psm1</td><td>.xip</td></tr>
<tr><td>.crd</td><td>.mcf</td><td>.pst</td><td>.xnk</td></tr>
<tr><td>.crds</td><td>.mda</td><td>.pvw</td><td></td></tr>
<tr><td>.crt</td><td>.mdb</td><td>.py</td><td></td></tr>
<tr><td>.csh</td><td>.mde</td><td>.pyc</td><td></td></tr>
<tr><td>.der</td><td>.mdt</td><td>.pyo</td><td></td></tr>
<tr><td>.dll</td><td>.mdw</td><td>.rb</td><td></td></tr>
<tr><td>.drv</td><td>.mdz</td><td>.rbw</td><td></td></tr>
<tr><td>.exe</td><td>.msc</td><td>.rdp</td><td></td></tr>
<tr><td>.fxp</td><td>.msh</td><td>.reg</td><td></td></tr>
<tr><td>.fon</td><td>.msh1</td><td>.rgu</td><td></td></tr>
<tr><td>.gadget</td><td>.msh1xml</td><td>.scf</td><td></td></tr>
<tr><td>.grp</td><td>.msh2</td><td>.scr</td><td></td></tr>
<tr><td>.hlp</td><td>.msh2xml</td><td>.shb</td><td></td></tr>
<tr><td>.hme</td><td>.mshxml</td><td>.shs</td><td></td></tr>
<tr><td>.hpj</td><td>.msi</td><td>.sys</td><td></td></tr>
<tr><td>.hta</td><td>.msp</td><td>.theme</td><td></td></tr>
<tr><td>.inf</td><td>.mst</td><td>.tmp</td><td></td></tr>
<tr><td>.ins</td><td>.msu</td><td>.tsk</td><td></td></tr>
<tr><td>.isp</td><td>.ocx</td><td>.ttf</td><td></td></tr>
</table>

## <a name="reserved-uri-scheme-names"></a>保留 URI 配置名稱

<table>
<tr><td>application.manifest</td><td>internetshortcut</td><td>ms-settings:network-mobilehotspot</td><td>shbfile</td></tr>
<tr><td>application.reference</td><td>javascript</td><td>ms-settings:network-proxy</td><td>shcmdfile</td></tr>
<tr><td>batfile</td><td>jscript</td><td>ms-settings:network-wifi</td><td>shsfile</td></tr>
<tr><td>Bing</td><td>jsefile</td><td>ms-settings:nfctransactions</td><td>smb</td></tr>
<tr><td>blob</td><td>ldap</td><td>ms-settings:notifications</td><td>stickynotes</td></tr>
<tr><td>callto</td><td>lnkfile</td><td>ms-settings:personalization</td><td>sysfile</td></tr>
<tr><td>cerfile</td><td>mailto</td><td>ms-settings:privacy-calendar</td><td>tel</td></tr>
<tr><td>chm.filecmdfilecomfile</td><td>maps</td><td>ms-settings:privacy-contacts</td><td>telnet</td></tr>
<tr><td>cplfile</td><td>microsoft.powershellscript.1</td><td>ms-settings:privacy-customdevices</td><td>tn3270</td></tr>
<tr><td>dllfile</td><td>ms-accountpictureprovider</td><td>ms-settings:privacy-feedback</td><td>ttffile</td></tr>
<tr><td>drvfile</td><td>ms-appdata</td><td>ms-settings:privacy-location</td><td>unknown</td></tr>
<tr><td>dtmf</td><td>ms-appx</td><td>ms-settings:privacy-messaging</td><td>usertileprovider</td></tr>
<tr><td>exefile</td><td>ms-autoplay</td><td>ms-settings:privacy-microphone</td><td>vbefile</td></tr>
<tr><td>explorer.assocactionid.burnselection</td><td>ms-excel</td><td>ms-settings:privacy-speechtyping</td><td>vbscript</td></tr>
<tr><td>explorer.assocactionid.closesession</td><td>msi.package</td><td>ms-settings:privacy-webcam</td><td>vbsfile</td></tr>
<tr><td>explorer.assocactionid.erasedisc</td><td>msi.patch</td><td>ms-settings:proximity</td><td>wallet</td></tr>
<tr><td>explorer.assocactionid.zipselection</td><td>ms-powerpoint</td><td>ms-settings:regionlanguage</td><td>windows.gadget</td></tr>
<tr><td>explorer.assocprotocol.search-ms</td><td>ms-settings</td><td>ms-settings:screenrotation</td><td>windowsmediacenterapp</td></tr>
<tr><td>explorer.burnselection</td><td>ms-settings:batterysaver</td><td>ms-settings:speech</td><td>windowsmediacenterssl</td></tr>
<tr><td>explorer.burnselectionexplorer.closesession</td><td>ms-settings:batterysaver-settings</td><td>ms-settings:storagesense</td><td>windowsmediacenterweb</td></tr>
<tr><td>explorer.closesession</td><td>ms-settings:batterysaver-usagedetails</td><td>ms-settings:windowsupdate</td><td>wmp11.assocprotocol.mms</td></tr>
<tr><td>explorer.erasedisc</td><td>ms-settings:bluetooth</td><td>ms-settings:workplace</td><td>wsffile</td></tr>
<tr><td>explorer.zipselection</td><td>ms-settings:connecteddevices</td><td>ms-windows-store</td><td>wsfile</td></tr>
<tr><td>file</td><td>ms-settings:cortanasearch</td><td>ms-word</td><td>wshfile</td></tr>
<tr><td>fonfile</td><td>ms-settings:datasense</td><td>ocxfile</td><td>xbls</td></tr>
<tr><td>hlpfile</td><td>ms-settings:dateandtime</td><td>office</td><td>zune</td></tr>
<tr><td>htafile</td><td>ms-settings:display</td><td>onenote</td><td></td></tr>
<tr><td>http</td><td>ms-settings:lockscreen</td><td>piffile</td><td></td></tr>
<tr><td>https</td><td>ms-settings:maps</td><td>regfile</td><td></td></tr>
<tr><td>iehistory</td><td>ms-settings:network-airplanemode</td><td>res</td><td></td></tr>
<tr><td>ierss</td><td>ms-settings:network-cellular</td><td>rlogin</td><td></td></tr>
<tr><td>inffile</td><td>ms-settings:network-dialup</td><td>scrfile</td><td></td></tr>
<tr><td>insfile</td><td>ms-settings:network-ethernet</td><td>scriptletfile</td><td></td></tr>
</table>