---
title: Azure uygulama DMZ'ler ile kullanmak için
titlesuffix: Azure Virtual Network
description: Trafik akışı senaryolarını test etmek için bir DMZ oluşturduktan sonra bu basit bir web uygulaması dağıtma
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 6753b3a76ff5d3e0266f238d8e354943dec694a8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60788930"
---
# <a name="sample-application-for-use-with-dmzs"></a>DMZ'ler ile kullanmak için örnek uygulama
[Güvenlik sınırı en iyi yöntemler sayfasına geri dönün][HOME]

Bu PowerShell komut dosyalarını, yükleme ve arka uç AppVM01 sunucusundan içerik ile ön uç IIS01 sunucudan bir html sayfası görüntüleyen basit bir web uygulaması ayarlama için IIS01 ve AppVM01 sunucularında yerel olarak çalıştırılabilir.

Bu uygulama DMZ örneklerin çoğu için basit bir test ortamı sağlar ve nasıl değişiklikleri uç noktaları, Nsg'ler, UDR ve güvenlik duvarı kuralları trafik akışları etkileyebilir.

## <a name="firewall-rule-to-allow-icmp"></a>ICMP izin vermek için güvenlik duvarı kuralı
Bu basit bir PowerShell ifadesi ICMP (Ping) trafiğine izin verecek şekilde bir Windows VM üzerinde çalıştırılabilir. (Varsayılan olarak açık ICMP çoğu Linux dağıtımları için) windows güvenlik duvarı üzerinden geçmek için daha kolay test etme ve sorun giderme ping Protokolü izin vererek bu Güvenlik Duvarı'nı güncelleştirme sağlar.

```powershell
# Turn On ICMPv4
New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
    -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
```

Aşağıdaki betikler kullanırsanız, bu güvenlik duvarı kuralı ekleme ilk açıklamadır.

## <a name="iis01---web-application-installation-script"></a>IIS01 - Web uygulama yükleme betiği
Bu betik olur:

1. IMCPv4 açın (Ping) daha kolay test edebilmek için yerel sunucu windows güvenlik duvarı
2. IIS ve .NET Framework v4.5
3. Bir ASP.NET web sayfası ve bir Web.config dosyası oluşturma
4. Dosya erişimi kolaylaştırmak için varsayılan uygulama havuzunu Değiştir
5. Anonim kullanıcı, yönetici hesabı ve parola ayarlayın.

RDP IIS01 içinde bulunduğu sırada bu PowerShell Betiği yerel olarak çalıştırılmalıdır.

```powershell
# IIS Server Post Build Config Script
# Get Admin Account and Password
    Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
    $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
    $thePassword = Read-Host -Prompt "The Admin Password"

# Turn On ICMPv4
    Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Install IIS
    Write-Host "Installing IIS and .NET 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
    add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console

# Create Web App Pages
    Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
    $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
    <%@ Import Namespace="System.IO" %>
    <script language="vb" runat="server">
        Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
            Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
            Dim objStreamReader As StreamReader
            objStreamReader = File.OpenText(FILENAME)
            Dim contents As String = objStreamReader.ReadToEnd()
            lblOutput.Text = contents
            objStreamReader.Close()
            lblTime.Text = Now()
        End Sub
    </script>

    <!DOCTYPE html>
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
        <title>DMZ Example App</title>
    </head>
    <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
      <form id="frmMain" runat="server">
        <div>
          <h1>Looks like you made it!</h1>
          This is a page from the inside (a web server on a private network),<br />
          and it is making its way to the outside! (If you are viewing this from the internet)<br />
          <br />
          The following sections show:
          <ul style="margin-top: 0px;">
            <li> Local Server Time - Shows if this page is or isn''t cached anywhere</li>
            <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
            <li> Image from the Internet - Doesn''t really show anything, but it made me happy to see this when the app worked</li>
          </ul>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Image File Linked from the Internet</b>:<br />
            <br />
            <img src="https://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
        </div>
      </form>
    </body>
    </html>'

    $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.web>
        <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
        <httpRuntime targetFramework="4.5" />
        <identity impersonate="true" />
        <customErrors mode="Off"/>
      </system.web>
      <system.webServer>
        <defaultDocument>
          <files>
            <add value="Home.aspx" />
          </files>
        </defaultDocument>
      </system.webServer>
    </configuration>'

    $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
    $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii

# Set App Pool to Classic Pipeline to remote file access will work easier
    Write-Host "Updating IIS Settings" -ForegroundColor Cyan
    c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
    c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost

# Make sure the IIS settings take
    Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
    Restart-Service -Name W3SVC

    Write-Host
    Write-Host "Web App Creation Successful!" -ForegroundColor Green
    Write-Host
```

## <a name="appvm01---file-server-installation-script"></a>AppVM01 - dosya sunucusu yükleme betiği
Bu betik bu basit bir uygulama için arka uç ayarlar. Bu betik olur:

1. IMCPv4 açın (Ping) daha kolay test edebilmek için güvenlik duvarı
2. Web sitesi için bir dizin oluşturun
3. Uzaktan Erişim tarafından web sayfasını olacak şekilde bir metin dosyası oluşturun
4. Dizin ve dosya anonim erişime izin vermek için izinleri ayarlayın
5. IE Artırılmış Güvenlik bu sunucudan daha kolay gezinme izin vermek için devre dışı bırakma

> [!IMPORTANT]
> **En iyi yöntem**: IE Artırılmış Güvenlik devre dışı bir üretim sunucusunda hiç kapatma yanı sıra genel olarak üretim sunucudan Web'de gezinmek için kötü bir fikir olduğunu. Ayrıca, dosya paylaşımlarına anonim erişim için açmayı bitti ancak kötü bir fikir kolaylık olması için aşağıdadır.
>
>

RDP AppVM01 içinde bulunduğu sırada bu PowerShell Betiği yerel olarak çalıştırılmalıdır. Başarılı yürütme emin olmak için yönetici olarak çalıştırılacak PowerShell gereklidir.

```powershell
# AppVM01 Server Post Build Config Script
# PowerShell must be run as Administrator for Net Share commands to work

# Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Create Directory
    New-Item "C:\WebShare" -ItemType Directory

# Write out Rand.txt
    $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
    $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii

# Set Permissions on share
    $Acl = Get-Acl "C:\WebShare"
    $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
    $Acl.SetAccessRule($AccessRule)
    Set-Acl "C:\WebShare" $Acl

# Create network share
    Net Share WebShare=C:\WebShare "/grant:Everyone,READ"

# Turn Off IE Enhanced Security Configuration for Admins
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0

    Write-Host
    Write-Host "File Server Set up Successful!" -ForegroundColor Green
    Write-Host
```

## <a name="dns01---dns-server-installation-script"></a>DNS01 - DNS sunucusu yükleme betiği
DNS sunucusu kurmak için bu örnek uygulama dahil hiçbir betik yoktur. Güvenlik duvarı kuralları, NSG veya UDR test DNS trafiği dahil etmek gerekiyorsa DNS01 sunucunun el ile ayarlanması gerekir. Ağ yapılandırma xml dosyasını ve Resource Manager şablonu için örneklerin her ikisi de birincil DNS sunucusu ve Düzey 3'ü yedekleme DNS sunucusu tarafından barındırılan ortak DNS sunucusu olarak DNS01 içerir. Düzey 3 DNS sunucusu yerel olmayan trafik için kullanılan gerçek DNS sunucusu ve ile DNS01 ayarlanmadı, DNS ortaya çıkabilecek herhangi bir yerel ağ.

## <a name="next-steps"></a>Sonraki adımlar
* Bir IIS sunucusunda IIS01 betiği çalıştırın
* Dosya sunucusu üzerinde AppVM01 Çalıştır
* Derleme doğrulamak için IIS01 genel IP için Gözat

<!--Link References-->
[HOME]: ../best-practices-network-security.md
